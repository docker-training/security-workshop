# Step 1 - `AppArmor` primer

The following command shows you how to check if AppArmor is enabled in your system's kernel:

`docker info | grep apparmor`{{execute}}

By default, Docker applies the `docker-default` AppArmor profile to new containers. This profile is located in ``/etc/apparmor.d/docker/`` and you can find more information about it in the [documentation](https://docs.docker.com/engine/security/apparmor/#understand-the-policies).

## Task

Check the `docker-default` AppArmor.

`cat /etc/apparmor.d/docker`{{execute}}

Here are some quick pointers for how to understand AppArmor profiles:

- `Include` statements, such as ``#include <abstractions/base>``, behave just like their `C` counterparts by expanding to additional AppArmor profile contents.

- AppArmor `deny` rules have precedence over allow and `owner `rules. This means that `deny` rules cannot be overridden by subsequent `allow` or `owner` rules for the same resource. Moreover, an `allow` will be overridden by a subsequent `deny` on the same resource

- For file operations, `r` corresponds to read, `w` to write, `k` to lock, `l` to link, and `x` to execute.

For more information, see the [official AppArmor documentation wiki](http://wiki.apparmor.net/index.php/Documentation) (under active development at this time).
