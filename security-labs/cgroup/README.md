# Docker Security Workshop: Control Groups (cgroups) lab

Control Groups, `cgroups` are a feature of the Linux kernel that allow you to limit the access processes and containers have to system resources such as CPU, RAM, IOPS and network.

In this lab you will use `cgroups` to limit the resources available to Docker containers. You will see how to pin a container to specific CPU cores, limit the number of CPU shares a container has, as well as how to prevent a fork bomb from taking down a Docker Host.

## You will complete the following steps in this lab:

- [Step 1 - `cgroups` and the Docker CLI](step1.md)
- [Step 2 - Max-out two CPUs](step2.md)
- [Step 3 - Set CPU affinity](step3.md)
- [Step 4 - CPU share constraints](step4.md)
- [Step 5 - `Docker Compose` and `cgroups`](step5.md)
- [Step 6 - Preventing a fork bomb](step6.md)
- [Summary](finish.md)
