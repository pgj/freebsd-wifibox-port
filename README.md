# FreeBSD Wifibox Port

This repository hosts the development of the FreeBSD port for the
[wifibox project](https://github.com/pgj/freebsd-wifibox) to provide
proper packaging.  Changes here are upstreamed to the FreeBSD ports
tree time to time.

*This is the development version of the port, use it only if the
[`net/wifibox`](https://cgit.freebsd.org/ports/tree/net/wifibox) port
from the [FreeBSD Ports
Collection](https://docs.freebsd.org/en/books/handbook/ports/#ports-using)
does not work for some reason.*

## Preperations

Depending on the major version of FreeBSD and the configuration
options, the port may depend on
[sysutils/bhyve+](https://github.com/pgj/freebsd-bhyve-plus-port/),
which may need to be installed first.  Nowadays that is part of the
ports tree, therefore it can be installed as a package:

```console
# pkg install sysutils/bhyve+
```

In case of older fresh installs, the ports tree may need to be updated
to its latest version to get the `sysutils/bhyve+` port, for example
by using `portsnap(8)`:

```console
# portsnap fetch update
```

For `net/wifibox-alpine`, it might be easier if the dependencies are
installed beforehand via packages.

```console
# pkg install gtar patchelf squashfs-tools-ng
```

Also, for building this port, make sure that the [Linuxulator] is
activated, for example, by loading the corresponding kernel module.
There is no need to install a full Linux base system (this is going to
be done for the port itself), but if that is already there it will not
cause a problem either.

```console
# kldload linux64
```

For `net/wifibox-core`, the default dependencies are follows, which
could be also installed via packages.  The `socat` package is only
required if the Unix Domain Socket pass-through is going to be
activated.

```console
# pkg install grub2-bhyve socat
```

## Installation

Once this repository is cloned and the necessary preparations are
made, the ports can be easily installed in the standard way.

```console
# make -C net/wifibox-alpine install clean
# make -C net/wifibox-core install clean
# make -C net/wifibox install clean
```

The `net/wifibox` is only a metaport, it is created only to make it
easier to install the other two ports, which are required on the other
hand.  There are also flavors available, which can be learned by the
following command.

```console
# make -C net/wifibox -V FLAVORS
```

Flavors matter only for the `wifibox` and `wifibox-alpine` ports and
they refer to the set of firmware files to be included in the guest
disk image.  For example, if ports should be built with the `iwlwifi`
firmware files only, just use the corresponding flavor.

```console
# make -C net/wifibox-alpine \
	FLAVOR=iwlwifi \
	install \
	clean
```

If there are many unused firmware files installed still, the
`FIRMWARE_FILES` variable can be used to cut down on their number to
list only the ones to be used.  This may produce a significantly
smaller guest disk image.

```console
# make -C net/wifibox-alpine \
	FLAVOR=iwlwifi \
	FIRMWARE_FILES=iwlwifi-8000C-36.ucode \
	install \
	clean
```

[Linuxulator]: https://docs.freebsd.org/en/books/handbook/linuxemu/
