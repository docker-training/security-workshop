**User namespaces** have been part of the Linux kernel for a while. They have been available in Docker since version 1.10 of the Linux Docker Engine. They allow the Docker daemon to create an isolated namespace that looks and feels like a root namespace. However, the `root` user inside of this namespace is mapped to a non-privileged `uid` on the Docker Host. This means that containers can effectively have root privilege inside of the user namespace, but have no privileges on the Docker Host.

In this step you'll see how to implement *user namespaces*.

> **Note:** It is not recommended to switch the Docker daemon back and forth between having user namespace mode enabled, and user namespace mode disabled. Doing this can cause issues with image permissions and visibility.

## Stop the Docker Daemon


`sudo systemctl stop docker `{{execute}}



## Start the Docker Daemon with user namespace support enabled.

`service docker status`{{execute}}

`sudo dockerd --userns-remap=default &`{{execute}}

> Note: If you are using a Docker 1.11 or earlier you will need to supplement dockerd with docker daemon in the previous command.


  This will start the Docker Daemon in the background using the default user namespace mapping where the **dockermap** user and group are created and mapped to non-privileged **uid** and **gid** ranges in the `/etc/subuid` and `/etc/subgid` files.

  The `/etc/subuid` and `/etc/subgid` files have a single-line entry for each user/group. Each line is formatted with three fields as follows:

  - User or group name
  - Subordinate user ID (uid) or group ID (gid)
  - Number of subordinate ID's available

  `sudo cat /etc/subuid`{{execute}}

  and

  `sudo cat /etc/subgid`{{execute}}

  This step has shown you that the Docker daemon runs as a user namespaces with **dockermap** user and group.


## Use the `docker info` command to verify that user namespace support is properly enabled.

  `docker info`{{execute}}

  The numbers at the end of the *Docker Root Dir* line indicate that the daemon is running inside of a user namespace. The numbers will match the subordinate user ID of the `dockermap` user as defined in the /etc/subuid file.


## Check which images are stored in the daemon's local store.

`sudo docker images`{{execute}}

The Docker daemon's local store is empty!

This is despite the fact that the alpine image was pulled locally in a previous step. This is because this instance of the Docker daemon is running inside of a brand new namespace and has no visibility of anything outside of that namespace.


## try running a new container in privileged mode.

`sudo docker run --rm --privileged alpine id`{{execute}}

As stated in the error response, privileged containers are not currently supported with user namespaces.

Start a new container in interactive mode and mount the Docker Host's /bin directory as a volume.

`sudo docker run -it --rm -v /bin:/host/bin busybox /bin/sh`{{execute}}

New image will be downloaded and run.

## Use the `id` command to verify the security context the container is running under.

The previous command should have attached you to the terminal of the newly created container.

The following commands should be ran from inside of the container created in the previous step.

`id`{{execute}}

The output above shows that the container is running under the root user's security context. Remember that this is only root within the scope of the namespace that the container is running in.

## Try and delete a file that exists in the volume that you mounted from the Docker Host's filesystem into the container as a volume.

`rm host/bin/sh`{{execute}}

The operation fails with a permission denied error. This is because the file you are trying to delete exists in the local filesystem of the Docker Host and the container does not have root access outside of the namespace that it exists in.

If you perform the same operation - start the same container and attempt the same rm command - without user namespace support enabled, the operation will succeed.

## Clean-up

The following commands will clean up the user namespace you've been working in and restart the Docker daemon without user namespace support enabled.

`sudo docker rm -f $(docker ps -aq)`{{execute}}

`sudo docker rmi $(docker images -q)`{{execute}}

`sudo killall dockerd -v`{{execute}}


Restart the Docker daemon without user namespace support enabled.

`sudo systemctl start docker`{{execute}}
