# Step 1 - Clone the labs GitHub repo

## Task 1 : check `seccomp` is enabled.

The following commands show you how to check if `seccomp` is enabled in your system's kernel:

   `docker info | grep seccomp`{{execute}}

If the above output does not return a line with `seccomp` then your system does not have seccomp enabled in its kernel.

Check from the Linux command line

   ``grep SECCOMP /boot/config-$(uname -r)``{{execute}}

Note: If you container doesn't have *seccomp* enabled, then you have to enabled *seccomp* in your linux kernel and then start your container.

## Seccomp and Docker

Docker has used `seccomp` since version 1.10 of the Docker Engine.

Docker uses seccomp in *filter* mode and has its own JSON-based DSL that allows you to define *profiles* that compile down to seccomp filters. When you run a container it gets the default seccomp profile unless you override this by passing the ``--security-opt`` flag to the `docker run` or `docker create` command.

The following example command starts an interactive container based off the Alpine image and starts a shell process. It also applies the `default.json` seccomp profile to it.

   ``sudo docker run -it --rm --security-opt seccomp=default.json alpine sh ..``

The above command sends the JSON file from the client to the daemon where it is compiled into a BPF program using a [thin Go wrapper around libseccomp](https://github.com/seccomp/libseccomp-golang).

Docker seccomp profiles operate using a whitelist approach that specifies allowed syscalls. _Only_ syscalls on the whitelist are permitted.

Docker supports many security related technologies. It is possible for other security related technologies to interfere with your testing of seccomp profiles. For this reason, the best way to test the effect of seccomp profiles is to add all capabilities and disable `apparmor`. This gives you the confidence the behavior you see in the following steps is solely due to `seccomp` changes.

For example, the following `docker run` flags add all capabilities and disable apparmor: ``--cap-add ALL --security-opt apparmor=unconfined``.


## Task 2: Clone the labs GitHub repo

In this step you will clone the lab's GitHub repo so that you have the seccomp profiles that you will use for the remainder of this lab.

Clone the labs GitHub repo.

`git clone https://github.com/docker-training/security-workshop.git`{{execute}}

Change into the security-workshop/seccomp directory.

`cd dsecurity-workshop/seccomp`{{execute}}

The remaining steps in this lab will assume that you are running commands from this `security-workshop/seccomp` directory. This will be important when referencing the seccomp profiles on the various docker run commands throughout the lab.
