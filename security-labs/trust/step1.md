# Step 1 - Pulling images by tag

The most common and basic way to pull Docker image is by `tag`. The is where you specify an image name followed by an alphanumeric tag. The image name and tag are separated by a colon ``:``.

For example:


`docker pull alpine:edge`

## Task


This command will pull the Alpine image tagged as `edge`. The corresponding image can be found here on [Docker Hub](https://hub.docker.com/r/_/alpine/).

If no tag is specified, Docker will pull the image with the `latest` tag.

Pull the Alpine image with the `edge` tag.

`docker pull alpine:edge`{{execute}}

You will see the message starts with:

```
edge: Pulling from library/alpine                                                                                              
2f12ed5a7535: Pull complete                                                                                                    
Digest: sha256:cd9c03c2d382fcf00c31dc1635445163ec185dfffb51242d9e097892b3b0d5b4
```
and ends with:

```
Status: Downloaded newer image for alpine:edge
```

Confirm that the pull was successful.

`docker images`{{execute}}

Run a new container from the image.

`docker run --rm -it alpine:edge sh`{{execute}}



Ping [www.docker.com](http://www.docker.com/) from the inside of `alpine:edge` container.

``ping www.docker.com``{{execute}}

Stop `ping` by typing `Ctlr C`.

Exit the container.

`exit`{{execute}}

In this step you pulled an image by tag and verified the pull by running a container from it.
