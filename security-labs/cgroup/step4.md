By default, all containers get an equal share of time executing on the Docker Host's CPUs. This allocation of time can be modified by changing the container’s CPU share weighting relative to the weighting of all other running containers.

The ``--cpu-shares`` flag takes a value from 0-1024. The default value is 1024, and a value of 0 will also default to 1024. If three containers are running and one has 1024 shares, while the other two have 512, the first container will get 50% of processor time while the other two will each get 25%. However, these shares are only enforced when CPU intensive tasks are running. When a container is not busy, other containers can use the free CPU time.

Shares of CPU time are balanced across all cores on a multi-core Docker Host.

In this step you will use the `docker run` command with the ``--cpu-shares`` flag to influence the amount of CPU time containers get. You will start one container (**container-1**) with 768 shares, and another container (**container-2**) with 256 shares. Each container will be locked to the first CPU on the system, and each will be running the `stress -c 2` process. **Container-1** will receive 75% of the CPU time whereas **container-2** will receive 25%.



## Task

Start the first container with 768 CPU shares.

`sudo docker run -d --name container-1 --cpuset-cpus 0 --cpu-shares 768 stress-cpu`{{execute}}


Start the second container with 256 CPU shares.

`sudo docker run -d --name container-2 --cpuset-cpus 0 --cpu-shares 256 stress-cpu`{{execute}}



Verify that both containers are running with the docker ps command.

  `sudo docker ps`{{execute}}



View the output of `htop`.

`htop`{{execute}}

Notice two things about the htop output. First, only a single CPU is being maxed out. Second, there are four `stress` processes running. The first two in the list equate to ~75% of CPU time, and the second two equate to ~25% of CPU time.

Stop and remove the `stresser` container.

`sudo docker stop container-1 && sudo docker rm container-1`{{execute}}
`sudo docker stop container-2 && sudo docker rm container-2`{{execute}}

It will take little while!

In this step you've seen how to use the ``--cpu-shares`` flag to influence the amount of CPU time containers get relative to other containers on the same Docker Host.

Feel free to experiment further by running more containers with different relative weights.
