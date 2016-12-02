# Step 5 - `Docker Compose` and `cgroups`

In this step you'll see how to set CPU affinities via Docker Compose files.

See this section of the [Docker Compose documentation](https://docs.docker.com/compose/compose-file/#cpu-shares-cpu-quota-cpuset-domainname-hostname-ipc-mac-address-mem-limit-memswap-limit-privileged-read-only-restart-shm-size-stdin-open-tty-user-working-dir) for more information on leveraging other cgroup capabilities using Docker Compose.

Make sure you are in the `dockercon-workshop/cgroups/cpu-stress` directory of the repo that you pulled in previous step.


## Task

Edit the `docker-compose.yml` file and add the `cpuset: '1' ` line as shown below:

```
cpu-stress:
 build: .
 cpuset: '1'
```


The above `docker-compose.yml` file will ensure that containers based from it will run on CPU core  2. You will obviously need a Docker Host with at least 2 CPU cores for this to work.


Bring the application up in the background.


`docker-compose up -d`{{execute}}


Run `htop` to see the effect of the cpuset parameter.


  `htop`{{execute}}

  The `htop` output above shows the container and it's two stress processes locked to CPU core 1 (`cpuset` in Docker Compose indexes CPU cores starting at 0 whereas `htop` indexes CPU cores starting at 1).

Quit `htop` by typing `q`.

Stop and remove the `cpustress_cpu-stress` container.

``docker stop cpustress_cpu-stress_1 && docker rm cpustress_cpu-stress_1``{{execute}}

  In this step you've seen how Docker Compose can set container CPU affinities. Remember that Docker Compose can also set CPU quotas and shares. See the [documentation](https://docs.docker.com/compose/compose-file/#cpu-shares-cpu-quota-cpuset-domainname-hostname-ipc-mac-address-mem-limit-memswap-limit-privileged-read-only-restart-shm-size-stdin-open-tty-user-working-dir) for more detail.
