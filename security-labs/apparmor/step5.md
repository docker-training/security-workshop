# Step 5 - Custom `AppArmor` profile

The [Panama Papers hack](https://en.wikipedia.org/wiki/Panama_Papers) exposed millions of documents from Mossack Fonseca, a Panamanian law firm.

A probable cause, as described by [Wordfence](https://www.wordfence.com/blog/2016/04/mossack-fonseca-breach-vulnerable-slider-revolution/) and other reports, was an unpatched WordPress plugin. In particular, it has been suggested that the Revolution Slider plugin contained buggy code that allowed another plugin to take its place by making an [unauthenticated AJAX call to the plugin](https://www.wordfence.com/wp-content/uploads/2016/04/Screen-Shot-2016-04-07-at-10.31.37-AM.png).

WordPress and its plugins run as PHP. This means an attacker could upload their own malicious plugin to start a shell on WordPress by simply sending a request to the PHP resource to run the malicious code and spin up the shell.

## Task 1

In this step we'll show how a custom AppArmor profile could have protected Dockerized WordPress from this attack vector.

Clone the lab's GitHub repo locally on your Docker Host and change into the `apparmor/wordpress` directory.

`sudo git clone https://github.com/docker-training/security-workshop.git`{{execute}}

If you have not already, `cd` into the lab's `wordpress` directory.

`cd security-workshop/security-labs/apparmor/wordpress`{{execute}}

List the files in the directory.

``ls -l``{{execute}}


View the contents of the Docker Compose YAML file.

`cat docker-compose.yml`{{execute}}

You can see that the file is describing a WordPress application with two services:

- wordpress: a wordpress container that wraps Apache PHP
- mysql: a database to store data

Bring the application up.

``sudo docker-compose up &``{{execute}}

Point your browser to the public IP of your lab instance on port 8080.

**Get IP address of your node** : You should have received the IP addresses of all of your lab instances from the instructor.



Select your language, click Continue and complete the information required on the Welcome page. Make sure you remember the username and password you choose.

Login with the username and password you created.

Click **Plugins** from the left-hand navigation pane and install a plugin.

You should notice that the **docker-default** AppArmor prfile does not restrict the plugin installation. We could be exposed to malicious plugins like in the Panama Papers incident!

## Task 2

In the next few steps you'll apply a new `Apparmor` profile to a new WordPress container. The purpose of this is to prevent malicious themes and plugins from being uploaded and installed on our WordPress instance.

Bring the WordPress application down.

Run this command from the shell of your Docker Host, not the shell of the `wordpress` container.

`sudo docker-compose down`{{execute}}

Add the `wparmor` profile to the `wordpress` service in the `docker-compose.yml` file.

```
wordpress:
image: endophage/wordpress:lean
links:
    - mysql
<SNIP>
security_opt:
    - apparmor=wparmor
mysql:
image: mariadb:10.1.10
environment:
    - MYSQL_DATABASE=wordpress
    - MYSQL_ROOT_PASSWORD=2671d40f658f595f49cd585db8e522cc955d916ee92b67002adcf8127196e6b2
ports:
    - "3306"
```

If you look closely at the output above, you will see that two new lines have been added above the start of the mysql service definition -

**security_opt**:

- ``apparmor=wparmor``

Edit the `wparmor` profile to deny every directory under ``/var/www/html/wp-content`` except for the `uploads` directory (which is used for media).

To do this, add the following three lines towards the bottom of the file where it says "**YOUR WORK HERE**".

```
deny /var/www/html/wp-content/plugins/** wlx,
deny /var/www/html/wp-content/themes/** wlx,
owner /var/www.html/wp-content/uploads/** rw,
```

The ``*`` wildcard is only for files at a single level. The ``**`` wildcard will traverse subdirectories.

Parse the `wparmor` profile.

`sudo apparmor_parser wparmor`{{execute}}

Bring the Docker Compose WordPress app back up.

``sudo docker-compose up &``{{execute}}


Verify that the app is up.

`sudo docker-compose ps`{{execute}}


Bring the application down.

``sudo docker-compose down``{{execute}}

Congratulations! You've secured a WordPress instance against adding malicious plugins :)
