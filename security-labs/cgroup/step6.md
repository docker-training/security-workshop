# Step 6 - Preventing a fork bomb
A _fork bomb_ is a form of denial of service (DoS) attack where a process continually replicates itself with the goal of depleting system resources to the point where a system can no longer function.

In this step you will use the ``--pids-limit`` flag to limit the number of processes a container can fork at runtime. This will prevent a fork bomb from consuming the Docker Host's entire process table.


## Task

Start a new container and limit the number of processes it can create to 200 with the following command.


`docker run -it --pids-limit 200 debian:jessie bash`{{execute}}

Do not proceed to the next step if you receive the following warning about pids limit support - "WARNING: Your kernel does not support pids limit capabilities, pids limit discarded". If you receive this warning, you can watch the demo as a [video](https://asciinema.org/a/dewdpjlzom4zasdz0c0c46jt9) instead.


Run the following command to create a fork bomb in the container you just started.

The previous command will have attached your terminal to the bash shell inside of the container created in the previous step.

**Do not complete this step if you received the error that your kernel does not support the pids limit feature...**


`:(){ :|: & };:`{{execute}}

Note: The command above is ``:(){ :|: & };:``.

You will need to press `Ctrl-C` to stop the process.

In this step you have seen how to set a process limits on a container that will prevent it from consuming all process table resources on the underlying Docker Host.
