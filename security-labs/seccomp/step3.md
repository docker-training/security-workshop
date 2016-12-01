# Step 3 - Run a container with no `seccomp` profile

Unless you specify a different profile, Docker will apply the [default seccomp profile](https://github.com/docker/docker/blob/master/profiles/seccomp/default.json) to all new containers. In this step you will see how to force a new container to run without a seccomp profile.


## Task

Start a new container with the ``--security-opt seccomp=unconfined`` flag so that no seccomp profile is applied to it.

`sudo docker run --rm -it --security-opt seccomp=unconfined debian:jessie sh`{{execute}}

From the terminal of the container run a `whoami` command to confirm that the container works and can make syscalls back to the Docker Host.

`whoami`{{execute}}

To prove that we are not running with the default seccomp profile, try running a `unshare` command, which creates a new namespace:

`unshare --map-root-user --user`{{execute}}

`whoami`{{execute}}

Exit the container.

> Note: If you don't have `strace` installed, you can installed with the the following commands: ```sudo apt-get update && sudo apt-get install strace```

Run the following `strace` command from your Docker Host to see a list of the syscalls used by the `whoami` program.

Your Docker Host will need the `strace` package installed.

``strace -c -f -S name whoami 2>&1 1>/dev/null | tail -n +3 | head -n -2 | awk '{print $(NF)}'``{{execute}}


You can also run the following simpler command and get a more verbose output.

`strace whoami`{{execute}}

You can substitute `whoami` for any other program.

In this step you started a new container with no seccomp profile and verified that the `whoami` program could execute. You also used the `strace` program to list the syscalls made by a particular run of the `whoami` program.
