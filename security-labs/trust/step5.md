# Step 5 - Extra for experts

Docker Content Trust is powered by [Notary](https://github.com/docker/notary), an open-source TUF-client and server that can operate over arbitrary trusted collections of data. Notary has its own CLI with robust features such as the ability to rotate keys and remove trust data. In this step you will play with the Notary CLI and a local instance of the Notary server instead of the one deployed alongside Docker Hub.

## Install notray
Run a service for testing or development
The quickest way to spin up a full Notary service for testing and development purposes is to use the Docker compose file in the Notary project.

```
$ git clone https://github.com/docker/notary.git
$ cd notary
$ docker-compose up
```

This will build the development Notary server and Notary signer images, and start up containers for the Notary server, Notary signer, and the MySQL database that both of them share. The MySQL data is stored in a volume.

Notary server and Notary signer communicate over mutually authenticated TLS (using the self-signed testing certs in the repository), and Notary server listens for HTTPS traffic on port 4443.

By default, this development Notary server container runs with the testing self-signed TLS certificates. In order to be able to successfully connect to it, you will have to use the root CA file in ``fixtures/root-ca.crt``.

For example, to connect using OpenSSL:

```
$ openssl s_client -connect <docker host>:4443 -CAfile fixtures/root-ca.crt -no_ssl3 -no_ssl2
```

To connect using the Notary Client CLI, please see Getting Started documentation. Please note that the version of Notary server and signer should be greater than or equal to that of the Notary Client CLI to ensure feature compatibility, i.e. if you are using Notary Client CLI 0.2, ensure you are using a server and signer tagged with an equal or higher version than 0.2 from the releases page.

## Get a notary client.

This can be done by downloading a binary directly from the [releases page](https://github.com/docker/notary/releases), or by cloning the notary repository into a valid Go repository structure (instructions at the end of the README) and building a client by running make binaries. If you build the notary binary yourself, it will be placed in the bin subdirectory within the notary git repo directory.

Use the notary client to inspect the existing Alpine repository on Docker Hub.

> IF you don't have `notray`, install with the command `sudo apt install notary `


`notary -s https://notary.docker.io -d ~/.docker/trust list docker.io/library/alpine`{{execute}}

Note that docker.io/ must be prepended to the image name for hub images. You should also try your own image you pushed to hub earlier!

clone the Notary repository.


`git clone https://github.com/docker/notary.git`{{execute}}

`cd notray`{{execute}}

Bring up a local notary server and signer.



  `docker-compose up -d`{{execute}}

  Add `127.0.0.1` notary-server to ``/etc/hosts`` (or if using docker-machine, add ``$(docker-machine ip) notary-server)``.


  Copy the config and certificate required to talk to your local Notary server.

  `mkdir -p ~/.notary && cp cmd/notary/config.json cmd/notary/root-ca.crt ~/.notary`{{execute}}


  Initialize a new trusted collection on your local server

  `notary init example.com/scripts`{{execute}}

  You will be prompted for passphrases for the root and repository keys. This is for the same reasons as it was when you first pushed a repo with Docker Content Trust enabled.

  Add content to your trusted collection by running a sequence of notary add example.com/scripts <NAME> <FILE>, notary publish example.com/scripts, and notary list example.com/scripts. This is a the sequence, in order:

  Stages adding a target with notary add
  Attempts to publish this target to the server with notary publish
  Fetches and displays all trust data for example.com (verifying output as it is downloaded)
  To remove targets, you can make use of the notary remove example.com/scripts <NAME> command, followed by a notary publish example.com/scripts.

  Please note that this docker-compose setup will persist notary's database data in a volume; if you'd like to wipe out all state when running this lab running multiple times, you will have to remove this volume in addition to restarting the notary server, signer, and database containers.

## Key rotation with the notary client

Let's look at key rotation with Notary.

In the event of key-compromise, it's a simple procedure to rotate keys with notary. Simply determine which key you wish to rotate, and run notary key rotate.

List all of the keys in your notary repository

`notary key list`{{execute}}

Rotate a key.

In this example we'll rotate the targets key.

`notary key rotate example.com/scripts targets`{{execute}}


Confirm that the targets key ID has changed.

`notary key list`{{execute}}


For more information about keys, please see the this section of the [Notary Service Architecture](https://docs.docker.com/notary/service_architecture/#brief-overview-of-tuf-keys-and-roles) documentation.



## Role delegation with notary

Now we'll explore delegation roles in notary. Delegation roles are a subset of the targets role, and are ideal for assigning signing privileges to collaborators and CI systems because no private key sharing is required. Here's a [demo of setting up delegation roles](https://asciinema.org/a/4nclzcuus3ubdcu88xmepz8u4) to illustrate the steps below:

Rotate the snapshot key to the server.

This is done by default when creating new Content Trust repositories in Docker 1.10+.

`notary key rotate example.com/scripts snapshot -r`{{execute}}

This is so that delegation roles will only require their own delegation's private key to publish to trusted collections.

Have your delegate generate an x509 certificate + private key pair with openssl - [instructions](https://docs.docker.com/engine/security/trust/trust_delegation/#generating-delegation-keys) here.

Retrieve their certificate, `delegation.crt`.

Add their delegation role.

`notary delegation add example.com/scripts targets/releases delegation.crt --all-paths`{{execute}}

This command will allow the collaborator to push any target (from ``--all-paths``) to the ``targets/releases`` role if they can sign with their private key `delegation.key` in order to produce a valid signature that can be verified by delegation.crt's public key material.

Be aware that this commmand only stages the delegation role addition.

Publish the addition of the delegation role

`notary publish example.com/scripts`{{execute}}

Check that the delegation role was added.

`notary delegation list example.com/scripts`{{execute}}

Your collaborator should now be able to publish content (`docker push`) with Docker Content Trust enabled, or with a `notary` add `example.com/scripts <NAME> <FILE> -r targets/releases`. You can verify their pushes by running a `notary list example.com/scripts -r targets/releases`

You can add additional keys to the same role with additional `delegation add` commands, like so: `notary delegation add example.com/scripts targets/releases delegation2.crt delegation3.crt`, followed by a publish.

For more commands over delegation roles, please consult the [notary advanced usage documentation](https://docs.docker.com/notary/advanced_usage/#work-with-delegation-roles).
