# User Namespaces

By default, the Docker daemon runs as `root`.
This allows the Docker daemon to create and work with the kernel structures required to start containers.
However, it also presents potential security risks. This lab will walk you through implementing a more secure configuration utilizing `user namespaces`.

##You will complete the following steps in this lab:

- Step 1 - Daemon and container defaults
- Step 2 - The ``--user`` flag
- Step 3 - Enabling `user namespaces`



## User Namespaces and Containers

It is useful for providing root access inside of a container. Imagine that the root user (uid 0) in container A maps to uid 1000 on the underlying system, and that root (uid 0) in container B maps to user id 2000 on the underlying system. This allows the administrator to give someone uid 0 (root) in the container without giving them uid 0 on the underlying system. It also allows a user to freely add/delete users inside the container.
