

## Summary

Congratulations. You've seen how to use some of the **capabilities** features supported by Docker and how to add and remove capabilities in new containers.




### Further reading:

This [article](https://www.kernel.org/doc/ols/2008/ols2008v1-pages-163-172.pdf) explains capabilities in a lot of detail. It will help you understand how capability sets interact with each other, and is very useful if you plan to run privileged docker containers and manage capabilities manually inside of them.

This is the [man page for capabilities](http://man7.org/linux/man-pages/man7/capabilities.7.html). Most of the complex interactions between capability sets don't affect Docker containers as long as there are no files with capability bits set.
