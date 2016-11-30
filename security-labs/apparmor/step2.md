In this step you will check the status of AppArmor on your Docker Host and learn how to identify whether or not Docker containers are running with an AppArmor profile.

## Task

View the status of AppArmor on your Docker Host with the apparmor_status command.

`sudo apparmor_status`{{execute}}

The command may require admin credentials.

Notice the **docker-default** profile is in enforce mode. This is the AppArmor profile that will be applied to new containers unless overridden with the ``--security-opts`` flag.

Run a new container and put it in the back ground.

`sudo docker run -dit alpine sh`{{execute}}

Confirm that the container is running.

`sudo docker ps`{{execute}}

Run the apparmor_status command again.

``sudo apparmor_status``{{execute}}


Notice that **docker-default** is now also listed under **processes are in enforcing mode**. The value in parenthesis is the PID of the container's process as seen from the PID namespace of the Docker Host. If you have multiple containers running you will see multiple docker-default policies listed under **processes in enforcing mode**.

Profiles in _enforce mode_ will actively deny operations based on the AppArmor profile. Profiles in _complain mode_ will log profile violations but will not block functionality.

Stop and remove the container started in the previous steps.

The example below uses the Container ID "1bb16561bc06". This will be different in your environment.

`sudo docker rm -f 1bb16561bc06`

In this step you learned how to check the AppArmor status on your Docker Host and how to check if a container is running with the **docker-default** AppArmor profile.
