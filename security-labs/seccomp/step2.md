# Step 2 - Test a `seccomp` profile

In this step you will use the `deny.json` seccomp profile included the lab guides repo. This profile has an empty syscall whitelist meaning all syscalls will be blocked. As part of the demo you will add all `capabilities` and effectively disable apparmor so that you know that only your seccomp profile is preventing the syscalls.



## Task

Use the docker run command to try to start a new container with all capabilities added, `apparmor` unconfined, and the `seccomp-profiles/deny.json` seccomp profile applied.


`sudo docker run --rm -it --cap-add ALL --security-opt apparmor=unconfined --security-opt seccomp=seccomp-profiles/deny.json alpine sh`{{execute}}




In this scenario, Docker doesn't actually have enough syscalls to start the container!

Inspect the contents of the `seccomp-profiles/deny/json` profile.

`cat seccomp-profiles/deny.json`{{execute}}


Notice that there are no `syscalls` in the whitelist. This means that no `syscalls` will be allowed from containers started with this profile.

In this step you removed capabilities and apparmor from interfering, and started a new container with a seccomp profile that had no syscalls in its whitelist. You saw how this prevented all syscalls from within the container or to let it start in the first place.
