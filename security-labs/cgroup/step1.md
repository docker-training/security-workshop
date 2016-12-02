# Step 1 - `cgroups` and the Docker CLI
Read man page for `cgroup`.

`man cgroup`{{execute}}

The `docker run` command provides many flags that allow you to apply `cgroup` limitations to new containers.

`docker run --help`{{execute}}

Scroll up and read description of the following flags which will be used in this lab:


| **Environment**  | **comments**                        |
|------------------|-------------------------------------|
| ``--cpu-shares``    | CPU shares (relative weight)        |
| ``--cpuset-cpus``    | CPUs in which to allow execution (0-3, 0,1)    |
| ``--pids-limit ``    | Tune container pids limit (set -1 for unlimited)        |

For more information on `cgroup` related flags available to the `docker run` command, see the [docker run reference guide](https://docs.docker.com/engine/reference/run/#specifying-custom-cgroups).
