Pulling by tag is easy and convenient. However, tags are mutable, and the same tag can refer to different images over time. For example, you can add updates to an image and push the updated image using the same tag as a previous version of the image. This scenario where a single tag points to multiple versions of an image can lead to bugs and vulnerabilities in your production environments.

This is why pulling by **digest** is such a powerful operation. Thanks to the content-addressable storage model used by Docker images, we can target pulls to specific image contents by pulling by digest. In this step you'll see how to pull by digest.




## Task

Pull the Alpine image with the **sha256:b7233dafbed64e3738630b69382a8b231726aa1014ccaabc1947c5308a8910a7** digest.


`sudo docker pull alpine@sha256:b7233dafbed64e3738630b69382a8b231726aa1014ccaabc1947c5308a8910a7`{{execute}}

Check that the pull succeeded.


``sudo docker images --digests alpine``{{execute}}

Notice that there are now two Alpine images in the Docker Hosts local repository. One lists the `edge` tag. The other lists ``<none>`` as the tag along with the ``b7233daf...`` digest.



The content addressable storage model used by Docker images means that any changes made to an image will result in a new digest for the updated image. This means it is not possible for two images with different contents to have the same digest.

In this step you learned how to pull images by digest and list image digests using the docker images command. For more information about the `docker pull` command, see the [documentation](https://docs.docker.com/engine/reference/commandline/pull/).
