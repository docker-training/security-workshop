
It's not easy to find the digest of a particular image tag. This is because it is computed from the hash of the image contents and stored in the image manifest. The image manifest is then stored in the Registry. This is why we needed a docker pull by tag to find digests previously. It would also be desirable to have additional security guarantees such as image freshness.

Enter Docker Content Trust: a system currently in the Docker Engine that verifies the publisher of images without sacrificing usability. Docker Content Trust implements The [Update Framework](https://theupdateframework.github.io/) (TUF), an NSF-funded research project succeeding Thandy of the Tor project. TUF uses a key hierarchy to ensure recoverable key compromise and robust freshness guarantees.

Under the hood, Docker Content Trust handles name resolution from IMAGE _tags_ to IMAGE _digests_ by signing its own metadata -- when Content Trust is enabled, docker will verify the _signatures_ and _expiration dates_ in the metadata before rewriting a pull by tag command to a pull by digest.

In this step you will enable Docker Content Trust, sign images as you push them, and pull signed and unsigned images.


## Task: DOCKER_CONTENT_TRUST

Enable Docker Content Trust by setting the `DOCKER_CONTENT_TRUST`environment variable.

`export DOCKER_CONTENT_TRUST=1`{{execute}}

Note: If you are using `sudo` with your Docker commands, you will need to preceded the above command so that it looks like this `sudo export DOCKER_CONTENT_TRUST=1`

It is worth nothing that although Docker Content Trust is now enabled, all Docker commands remain the same. Docker Content Trust will work silently in the background.


Pull the `riyaz/dockercon:trust` signed image.

`sudo docker pull riyaz/dockercon:trust`{{execute}}

Look closely at the output of the docker pull command and take particular notice of the name translation - how the command is translated to the digest as shown below:

```
trust: Pulling from riyaz/dockercon
fae91920dcd4: Pull complete
Digest: sha256:88a7163227a54bf0343aae9e7a4404fdcdcfef8cc777daf9686714f4376ede46
Status: Downloaded newer image for riyaz/dockercon:trust  
```

Pull and unsigned image.

`sudo docker pull riyaz/dockercon:untrust`{{execute}}

You cannot pull unsigned images with Docker Content Trust enabled. Once Docker Content Trust is enabled you can only pull, run, or build with trusted images.

```
Pulling repository docker.io/riyaz/dockercon                                                                      
Tag untrust not found in repository docker.io/riyaz/dockercon   
```

## Task: The Update Framework (TUF)

Login into docker hub, with your docker id.

`docker login`{{execute}}

> If you don't have docker hub accout, please register at [docker hub](https://hub.docker.com/register/)

Replace ``<your-docker-id>`` and execute the following to tag on your dockerhub repo.

Tag an ` alpine:trusted` image into a repository `docker.io/<your-docker-id>`

`sudo docker tag alpine:edge <your-docker-id>/alpine:trusted`{{execute}}


This will give you digest
```
The push refers to a repository [docker.io/icho/alpine]
6f4ada5745cd: Mounted from library/alpine
trusted: digest: sha256:cd9c03c2d382fcf00c31dc1635445163ec185dfffb51242d9e097892b3b0d5b4 size: 528
```

Push to a repository [docker.io/icho/alpine]

`sudo docker push <your-docker-id>/alpine:trusted`{{execute}}


Tag and push your own signed image with Docker Content Trust.

```
$ docker push icho/alpine:trusted
The push refers to a repository [docker.io/icho/alpine]
6f4ada5745cd: Mounted from library/alpine
trusted: digest: sha256:cd9c03c2d382fcf00c31dc1635445163ec185dfffb51242d9e097892b3b0d5b4 size: 528
Signing and pushing trust metadata
You are about to create a new root signing key passphrase. This passphrase
will be used to protect the most sensitive key in your signing system. Please
choose a long, complex passphrase and be careful to keep the password and the
key file itself secure and backed up. It is highly recommended that you use a
password manager to generate the passphrase and keep it safe. There will be no
way to recover this key. You can find the key in your config directory.
Enter passphrase for new root key with ID 4041d90:
Passphrase is too short. Please use a password manager to generate and store a good random passphrase.
Enter passphrase for new root key with ID 4041d90:
Passphrase is too short. Please use a password manager to generate and store a good random passphrase.
Enter passphrase for new root key with ID 4041d90:
Passphrase is too short. Please use a password manager to generate and store a good random passphrase.
Enter passphrase for new root key with ID 4041d90:
Passphrase is too short. Please use a password manager to generate and store a good random passphrase.
Enter passphrase for new root key with ID 4041d90:
Passphrase is too short. Please use a password manager to generate and store a good random passphrase.
Enter passphrase for new root key with ID 4041d90:
Repeat passphrase for new root key with ID 4041d90:
Passphrases do not match. Please retry.
Enter passphrase for new root key with ID 4041d90:
Repeat passphrase for new root key with ID 4041d90:
Enter passphrase for new repository key with ID 688a5af (docker.io/icho/alpine):
Repeat passphrase for new repository key with ID 688a5af (docker.io/icho/alpine):
Finished initializing "docker.io/icho/alpine"
Successfully signed "docker.io/icho/alpine":trusted
```

This command will prompt you for passphrases. This is because Docker Content Trust is generating a hierarchy of keys with different signing roles. Each key is encrypted with a passphrase, and it is best practice is to provide different passphrases for each key.

The **root key** is the most important key in TUF as it can rotate any other key in the system. The root key should be kept offline, ideally in hardware crypto device. It is stored in ``~/.docker/trust/private/root_keys`` by default.

The **tagging key** is the only local key required to push new tags to an existing repo, and is stored in ``~/.docker/trust/private/tuf_keys`` by default.

Feel free to explore the ``~/.docker/trust`` directory to view the internal metadata and key information that Docker Content Trust generates.

```
~/.docker$ tree
.
├── config.json
└── trust
    ├── private
    │   ├── root_keys
    │   │   └── 4041d9098c09360ddfb1585f29ec52b72c6d9b6a1001287261d5c3842ced1e90.key
    │   └── tuf_keys
    │       └── docker.io
    │           └── icho
    │               └── alpine
    │                   └── 688a5af45359e399e0ecc178f1b5c74866e8e929d516c0046e13f3ae71b0257f.key
    └── tuf
        └── docker.io
            └── icho
                └── alpine
                    ├── changelist
                    └── metadata
                        ├── root.json
                        └── targets.json

```

Disable Docker Content Trust.


`export DOCKER_CONTENT_TRUST=0`{{execute}}

Pull the image you just pushed in the previous step.

`sudo docker pull <your-docker-id>/alpine:trusted`{{execute}}

```
$ docker pull icho/alpine:trusted
trusted: Pulling from icho/alpine
Digest: sha256:cd9c03c2d382fcf00c31dc1635445163ec185dfffb51242d9e097892b3b0d5b4
Status: Image is up to date for icho/alpine:trusted
```

Enable Docker Content Trust.

`export DOCKER_CONTENT_TRUST=1`{{execute}}

Pull the same image again.

`sudo docker pull <your-docker-id>/alpine:trusted`{{execute}}

```
$ docker pull icho/alpine:trusted
Pull (1 of 1): icho/alpine:trusted@sha256:cd9c03c2d382fcf00c31dc1635445163ec185dfffb51242d9e097892b3b0d5b4
sha256:cd9c03c2d382fcf00c31dc1635445163ec185dfffb51242d9e097892b3b0d5b4: Pulling from icho/alpine
Digest: sha256:cd9c03c2d382fcf00c31dc1635445163ec185dfffb51242d9e097892b3b0d5b4
Status: Image is up to date for icho/alpine@sha256:cd9c03c2d382fcf00c31dc1635445163ec185dfffb51242d9e097892b3b0d5b4
Tagging icho/alpine@sha256:cd9c03c2d382fcf00c31dc1635445163ec185dfffb51242d9e097892b3b0d5b4 as icho/alpine:trusted
```

Take note of the difference between the pull of a signed image with Docker Content Trust enabled and disabled. With Docker Content Trust enabled the image pull is converted from a tagged image pull to a digest image pull.

In this step you have seen how to enable and disable Docker Content Trust. You have also seen how to sign images that you push. For more information about Docker Content Trust, see the [documentation](https://docs.docker.com/engine/security/trust/).
