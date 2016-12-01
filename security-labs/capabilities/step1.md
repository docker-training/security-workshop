#Step 1 - Introduction to capabilities


In this step you'll learn the basics of **capabilities**.

The Linux kernel is able to break down the privileges of the root user into distinct units referred to as **capabilities**. For example, the `CAP_CHOWN` capability is what allows the root use to make arbitrary changes to file UIDs and GIDs. The `CAP_DAC_OVERRIDE` capability allows the root user to bypass kernel permission checks on file read, write and execute operations. Almost all of the special powers associated with the Linux root user are broken down into individual capabilities.

This breaking down of `root` privileges into granular capabilities allows you to:

1. Remove individual capabilities from the `root` user account, making it less powerful/dangerous.
2. Add privileges to `non-root` users at a very granular level.

For example, Capabilities turn root and non-root privileges into a fine-grained access control system. Processes (like web servers) that just need to bind on a port below 1024 do not have to run as root: they can just be granted the `net_bind_service` capability instead. And there are many other capabilities, for almost all the specific areas where root privileges are usually needed.

Containers will not need “real” `root` privileges at all, and containers can run with a reduced capability set; meaning that `root` within a container has much less privileges than the real `root`.

Docker supports the addition and removal of capabilities, allowing use of a non-default profile. This may make Docker more secure through capability removal, or less secure through the addition of capabilities. The best practice for users would be to remove all capabilities except those explicitly required for their processes.

Capabilities apply to both files and threads. File capabilities allow users to execute programs with higher privileges. This is similar to the way the `setuid` bit works. Thread capabilities keep track of the current state of capabilities in running programs.

The Linux kernel lets you set capability `bounding sets` that impose limits on the capabilities that a `file/thread` can gain.

Docker imposes certain limitations that make working with capabilities much simpler. For example, `file capabilities` are stored within a file's extended attributes, and extended attributes are stripped out when Docker images are built. This means you will not normally have to concern yourself too much with file capabilities in containers.

> Note: It is of course possible to get file capabilities into containers at runtime, however this is not recommended.

In an environment without file based capabilities, it's not possible for applications to escalate their privileges beyond the `bounding set` (a set beyond which capabilities cannot grow). Docker sets the `bounding set` before starting a container. You can use Docker commands to add or remove capabilities to or from the `bounding set`.

By default, Docker drops all capabilities except [those needed](https://github.com/docker/docker/blob/master/oci/defaults_linux.go#L64-L79), using a whitelist approach.
