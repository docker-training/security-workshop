# Security Workshop
- Repository for Docker Security Workshop Hands on Labs.

## Lab Environment

### Platform Supported:

|  Release   |  OS   | Docker |
|---:|:----:|:-----|
|NAME|Ubuntu| Docker |
|VERSION|16.04 LTS (Xenial Xerus)| 1.12|

### Software Tools Enabled:

|     |  Tools  |
|---:|:----|
|Linux| seccomp, appamor|
|Docker| docker-compose|
|Misc.| apt-get, strace, htop|

### Pre-requisites

- Lgoin account to docker hub

## Lab Outline



| Lab Name | Level | Duration | Description |
|------:|:----:|:---:|:----------|
|cgroup | Intermediate | 20 min | This lab will walk you use `cgroups` to limit the resources available to Docker containers. You will see how to pin a container to specific CPU cores, limit the number of CPU shares a container has, as well as how to prevent a fork bomb from taking down a Docker Host. |
| User namespace | Intermediate | 10 min | This lab will walk you through implementing a more secure configuration utilizing user namespaces. |
| Content and Trust | Intermediate | 40 min | This lab focuses on understanding and securing image distribution. You'll start with a simple `docker pull` and build up to using Docker Content Trust (DCT).|
| Capabilities | Intermediate | 30 min | In this lab you'll learn the basics of `capabilities` in the Linux kernel. You'll learn how they work with Docker, some basic commands to view and manage `capabilities`, as well as how to add and remove capabilities in new containers. |
| seccomp |  Intermediate | 30 min | seccomp (short for secure computing mode) is a sandboxing facility in the Linux kernel that acts like a firewall for system calls (syscalls). You will learn how `Seccomp` can limit a containers access to the Docker Host's Linux kernel. |
| Appamor |  Intermediate | 30 min | You will learn how `AppArmor` can protect a Docker Host even when other lines of defense such as seccomp and Capabilities are not effective.|
