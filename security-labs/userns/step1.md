# Step 1 - Daemon and container defaults

In this step you'll verify that the Docker daemon, and containers, run by default as `root`. You will also force a single container to run under a different security context.

You must perform this step while logged in as the **ubuntu** user.

## Docker Daemon user

Use the `ps` command to verify that the *Docker daemon* is currently running under the **root** user's security context by running a command:

`ps aux | grep dockerd`{{execute}}

You see the 2 **dockerd** is listed.

The first line shows the Docker daemon (**dockerd**), running as a `root` user.

```
root      8715  0.0  1.0 352332 38820 ?        Ssl  12:56   0:01 /usr/bin/dockerd -H fd://
```

The second line shows the `ps` command you just ran as `ubuntu` user.

```
ubuntu    1669  0.0  0.1  10460   904 pts/0    S+   04:49   0:00 grep dockerd
```

 *Docker daemon* is running as **root** user.

## New Container user.

`id` command prints real and effective user and group IDs.

`id`{{execute}}

You will see *ubuntu* user id, group id and more.

Start a new container that runs the `id` command.

`sudo docker run --rm alpine id`{{execute}}


You see the followings in the last line output:

```
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel),11(floppy),20(dialout),26(tape),27(video)
```
The last line of the output above shows that the container is running as root - `uid=0(root)` and `gid=0(root)`.

This step has shown you that the Docker daemon runs as **root** by default. You have also seen that new containers also start as **root**.
