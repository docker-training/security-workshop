# Step 2 - The ``--user`` flag

In this step you will start a new container and force it to run under the security context of the user that you are logged in as.

You should be logged in as the **ubuntu** user.



## Find the current `uid` and `gid`

Type the `id` command form the terminal of your Docker Host to determine the `uid` and `gid` of the user you are currently logged in as.

`id`{{execute}}


The `uid` and `gid` in the above output are both "1000". Yours might be different. You will need your values in the next command.
```
uid=1000(ubuntu) gid=1000(ubuntu) groups=1000(ubuntu),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),3
0(dip),44(video),46(plugdev),102(netdev),999(docker)  
```

## Use ``--user`` flag

Use the docker run command with the ``--user`` flag to force a new container to start with the `uid` and `gid` of your current user.


`sudo docker run --rm --user 1000:1000 alpine id`{{execute}}


  The output shows that the container is running under `uid` 1000 and `gid` 1000.

```
uid=1000 gid=1000
```
  This proves that the container ran under the security context of a user and group that you specified.

  There are many times when containers need to run under the root security context, while at the same time not having root access to the entire Docker Host. This is where **user namespaces** help!
