# Step 4 - Selectively remove syscalls

In this step you will see how applying changes to the `default.json` profile can be a good way to fine-tune which syscalls are available to containers.

The `default-no-chmod.json` profile is a modification of the `default.json` profile with the ``chmod()``, ``fchmod()``, and ``chmodat()`` syscalls removed from its whitelist.


## Task

Start a new container with the `default-no-chmod.json` profile and attempt to run the `chmod 777 / -v`command.

`cd /home/ubuntu/dockercon-workshop/seccomp/seccomp-profiles`{{execute}}


`sudo docker run --rm -it --security-opt seccomp=default-no-chmod.json alpine sh`{{execute}}

From the container, try run the followings;

`chmod 777 / -v`{{execute}}


```
 / # chmod 777 / -v
chmod: /: Operation not permitted
```

The command fails because the `chmod 777 / -v` command uses some of the ``chmod()``, ``fchmod()``, and ``chmodat()`` syscalls that have been removed from the whitelist of the ``default-no-chmod.json`` profile.

Exit the container.

`exit`{{execute}}

Start another new container with the default.json profile and run the same chmod 777 / -v.

`sudo docker run --rm -it --security-opt seccomp=default.json alpine sh`{{execute}}

The command succeeds this time because the default.json profile has the chmod(), fchmod(), and chmodat syscalls included in its whitelist.

Exit the container.

`exit`{{execute}}

Check both profiles for the presence of the ``chmod()``, ``fchmod()``, and ``chmodat()`` syscalls.

Be sure to perform these commands from the command line of you Docker Host and not from inside of the container created in the previous step.

`cat ./seccomp-profiles/default.json | grep chmod`{{execute}}


`cat ./seccomp-profiles/default-no-chmod.json | grep chmod`{{execute}}

The output above shows that the `default-no-chmod.json` profile contains no chmod related syscalls in the whitelist.

In this step you saw how removing particular syscalls from the `default.json` profile can be a powerful way to start fine tuning the security of your containers.
