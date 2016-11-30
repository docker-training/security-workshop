

## Summary

In this lab you learned how to start the Docker daemon with user namespace support enabled. This started the daemon in a new namespace and mapped the root user inside of the namespace to a non-privileged user outside of the user namespace. This meant that the root user within the user namespace had full access to processes and containers within that namespace, but did not have elevated permissions outside of the namespace.

You proved that the Docker daemon was running within a user namespace using the `docker info` command. You saw that the root user inside of a the user namespace was unable to delete files that existed outside of the namespace.

You've completed your security lab: User Namespaces scenario!

## Additional Resources

You can refer to the following resources for more information and help:
- Docker: http://www.docker.com
