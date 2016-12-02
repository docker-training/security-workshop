# Step 4 - `AppArmor` and defense in depth

Defense in depth is a model where multiple different lines of defense work together to provide increased overall defensive capabilities. Docker uses AppArmor, seccomp, and Capabilities to form a deep defense system.

In this step you will see how AppArmor can protect a Docker Host even when other lines of defense such as `seccomp` and `Capabilities` are not effective.

## Task
If you haven't already done so, stop and remove all containers on your system.

``sudo docker rm -f $(docker ps -aq)``{{execute}}


Start a new Ubuntu container with seccomp disabled and the SYS_ADMIN capability added.

`sudo docker run --rm -it --cap-add SYS_ADMIN --security-opt seccomp=unconfined ubuntu sh`{{execute}}


This command will start a new container with the **default-docker** AppArmor profile automatically attached. seccomp will be disabled and the CAP_SYS_ADMIN capability added. This means that AppArmor will be the only effective line of defense for this container.

Make two new directories and bind mount them as shown below.

Run this command from within the container you just created.

`mkdir 1; mkdir 2; mount --bind 1 2`{{execute}}


The operation failed because the **default-docker** AppArmor profile denied the operation.

Exit the container.

`exit`{{execute}}

Confirm that it was the default-docker AppArmor profile that denied the operation by starting a new container without an AppArmor profile and retrying the same operation.

``docker run --rm -it --cap-add SYS_ADMIN --security-opt seccomp=unconfined --security-opt apparmor=unconfined ubuntu sh``{{execute}}


``mkdir 1; mkdir 2; mount --bind 1 2``{{execute}}


``ls -l``{{execute}}



The operation succeeded this time. This proves that it was the **default-docker** AppArmor profile that prevented the operation in the previous attempt.

Exit the container.

`exit`{{execute}}

In this step you have seen how AppArmor works together with seccomp and Capabilities to form a defense in depth security model for Docker. You saw a scenario where even with seccomp and Capabilities not preventing an action, AppArmor still prevented it.
