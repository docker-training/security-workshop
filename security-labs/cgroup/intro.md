# Control Groups: cgroups

Control Groups, `cgroups` are a feature of the Linux kernel that allow you to limit the access processes and containers have to system resources such as CPU, RAM, IOPS and network.

In this lab you will use `cgroups` to limit the resources available to Docker containers. You will see how to pin a container to specific CPU cores, limit the number of CPU shares a container has, as well as how to prevent a fork bomb from taking down a Docker Host.

## You will complete the following steps in this lab:

- Step 1 - `cgroups` and the Docker CLI
- Step 2 - Max-out two CPUs
- Step 3 - Set CPU affinity
- Step 4 - CPU share constraints
- Step 5 - `Docker Compose` and `cgroups`
- Step 6 - Preventing a fork bomb
