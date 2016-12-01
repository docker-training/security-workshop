# Step 2 - Max-out two CPUs

In this step you'll start a new container that will max out two CPU cores. You will need `htop` installed on your Docker Host, and you will need to be running a Docker Host that has two or more CPU cores. This step will still work if your Docker Host only has a single core. However, some of the `htop` outputs will be slightly different.


## Setup

We will use `htop` tool in this lab.

`htop`{{execute}}

Exit `htop`, by type in `q`.

> Note: If you haven't already done so, install `htop` on your Docker Host.


`sudo apt-get install htop`

Verify how many CPU Cores are used in this node.

`lscpu`{{execute}}

You will see 2 CPU cores, `` CPU(s):                2 ``

Clone the lab's GitHub repo locally on your Docker Host and change into the `cgroups/cpu-stress` directory.


`sudo git clone https://github.com/docker-training/security-workshop.git`{{execute}}

`cd security-workshop/cgroups/cpu-stress`{{execute}}


TODO: Note to lab owner. We might want to move this repo to a more official place along with the rest of the HOL guides and then update this doc accordingly.

## `Dockerfile` and `docker-compose.yaml`

Verify that the directory has a single `Dockerfile` and a single `docker-compose.yml` file.

`ls -l`{{execute}}

Inspect the contents of the Dockerfile.

`cat Dockerfile`{{execute}}

As you can see, the `Dockerfile` describes a simple Ubuntu-based container that runs a single command stress -c 2. This command spawns two processes - both spinning on sqrt(). The effect of this command is to stress two CPU cores.

## Build the image specified in the `Dockerfile`.

`sudo docker build -t stress-cpu .`{{execute}}

Run a new container called **stresser** based on the image built in the previous step.
Be sure to run the container in the background with the ``-d`` flag so that you can run the `htop` command in the next step from the same terminal.

`sudo docker run -d --name stresser stress-cpu`{{execute}}

It will take little while!

View the impact of the container using the `htop` command.

`htop`{{execute}}

Verify that the output shows two stress processes ``(stress -c 2)`` maxing out two of the CPUs on the system (CPU 1 and CPU 2). Both stress processes are in the running state, and both consuming 100% of the CPU they are executing on.

Stop and remove the `stresser` container.

`sudo docker stop stresser && sudo docker rm stresser`{{execute}}

It will take little while!


You have seen how it is possible for a single container to max out CPU resources on a Docker Host. You would max out your entire Docker Host if you were to start a stress worker processes for each CPU core.
