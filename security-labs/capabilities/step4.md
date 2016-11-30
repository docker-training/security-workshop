
The remainder of this lab will show you additional tools for working with capabilities form the Linux shell.

There are two main sets of tools for managing capabilities:

- **libcap** focuses on manipulating capabilities.
- **libcap-ng** has some useful tools for auditing.

Below are some useful commands from both.

You may need to manually install the packages required for some of these commands.

### libcap

- `capsh` - lets you perform capability testing and limited debugging
- `setcap` - set capability bits on a file
- `getcap` - get the capability bits from a file

### libcap-ng

- `pscap` - list the capabilities of running processes
- `filecap` - list the capabilities of files
- `captest` - test capabilities as well as list capabilities for current process

The remainder of this step will show you some examples of libcap and libcap-ng.

### Listing all capabilities

The following command will start a new container using Alpine Linux, install the libcap package and then list capabilities.

   ``sudo docker run --rm -it alpine sh -c 'apk add -U libcap; capsh --print'``{{execute}}


**Current** is multiple sets separated by spaces. Multiple capabilities within the same set are separated by commas ``,``. The letters following the ``+`` at the end of each set are as follows:

- `e` is effective
- `i` is inheritable
- `p` is permitted

For information on what these mean, see the [capabilities](http://man7.org/linux/man-pages/man7/capabilities.7.html) manpage.

### Experimenting with capabilities

The `capsh` command can be useful for experimenting with capabilities. capsh --help shows how to use the command:

`sudo capsh --help`{{execute}}

> Warning: ``--drop`` sounds like what you want to do, but it only affects the bounding set. This can be very confusing because it doesn't actually take away the capability from the effective or inheritable set. You almost always want to use ``--caps``.

### Modifying capabilities

`libcap` and `libcap-ng` can both be used to modify capabilities.

- Use `libcap` to modify the capabilities on a file.
The command below shows how to set the ``CAP_NET_RAW`` capability as effective and permitted on the file represented by ``$file``.
The `setcap` command calls on `libcap` to do this.

`setcap cap_net_raw=ep $file`{{execute}}

-  Use libcap-ng to set the capabilities of a file.
The filecap command calls on libcap-ng.

`filecap /absolute/path net_raw`{{execute}}

Note: `filecap` requires absolute path names. Shortcuts like ./ are not permitted.

### Auditing

There are multiple ways to read out the capabilities from a file.

Using `libcap`:

`getcap $file`{{execute}}


Using `libcap-ng`:

`filecap /absolue/path/to/file`{{execute}}


Using extended attributes (attr package):

`getfattr -n security.capability $file`{{execute}}


### Tips

Docker images cannot have files with capability bits set. This reduces the risk of Docker containers using capabilities to escalate privileges. However, it is possible to mount volumes that contain files with capability bits set into containers. Therefore you should use caution if doing this.

You can audit directories for capability bits with the following commands:

``getcap -r /``{{execute}}

``filecap -a``{{execute}}

To remove capability bits you can use.

``setcap -r $file``{{execute}}

``filecap /path/to/file none``{{execute}}
