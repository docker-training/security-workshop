
Docker makes it possible to restrict containers to a particular CPU core, or set of CPU cores. In this step you'll see how to restrict a container to a single CPU core using docker run with the ``--cpuset-cpus`` flag.

## Task

Run a new Docker container called `stresser` and restrict it to running on the first CPU on the system.


`sudo docker run -d --name stresser --cpuset-cpus 0 stress-cpu`{{execute}}

The ``--cpuset-cpus`` flag indexes CPU cores starting at 0. Therefore, CPU 0 is the first CPU on the system. You can specify multiple CPU cores as 0-4, 0,3 etc.


Run the `htop` command to see the impact the container is having on the Docker Host.


`htop`{{execute}}


There are a few things worth noting about what you have just done:

- The container is based on the same **stress-cpu** image that spawns two stress worker processes.
- The two stress worker processes have been restricted to running against a single CPU core by the ``--cpuset-cpus`` flag.
- Each of the two stress processes is consuming ~50% of available time on the single CPU core they are executing on.
- `htop` indexes CPU cores starting at 1 whereas ``--cpuset-cpus`` indexes starting at 0.

Stop and remove the `stresser` container.

`sudo docker stop stresser && sudo docker rm stresser`{{execute}}

It will take little while!

You have seen how to lock a container to a executing on a single CPU core on the Docker Host. Feel free to experiment further with the ``--cpuset-cpus`` flag.
