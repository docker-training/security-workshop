# Step 3 - Running a Container without an `AppArmor` profile

In this step you will see how to run a new container without an AppArmor profile.

## Task

If you haven't already done so, stop and remove all containers on your system.

``sudo docker rm -f $(docker ps -aq)``{{execute}}

Use the ``--security-opt apparmor=unconfined`` flag to start a new container in the background without an AppArmor profile.

`sudo docker run -dit --security-opt apparmor=unconfined alpine sh`{{execute}}

Confirm that the container is running.

`sudo docker ps`{{execute}}


Use the ``apparmor_status`` command to verify that the new container is not running with an AppArmor profile.

`sudo apparmor_status`{{execute}}

Notice that there are no instances of the **docker-default** AppArmor profile loaded under the **processes in enforce mode** section. This means that the container you just started does not have the **docker-default** AppArmor profile attached to it.

Stop and remove the container started in the previous steps.

The example below uses the Container ID "ace79581a19a". This will be different in your environment.

`sudo docker rm -f ace79581a19a`

In this step you learned that the ``--security-opt apparmor=unconfined`` flag will start a new container without an AppArmor profile.
