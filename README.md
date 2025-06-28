# FreeBSD Wifibox Port

This repository hosts the development of the FreeBSD port for the
[Wifibox project] to provide proper packaging.  Changes here are
downstreamed to the FreeBSD ports tree time to time.

*This is the development version of the port, use it only if the
[`net/wifibox`](https://cgit.freebsd.org/ports/tree/net/wifibox) port
from the [FreeBSD Ports Collection] does not work for some reason.
Please report issues at the [Wifibox project] directly.*

## Preperations

The installation process described here assumes that the [FreeBSD
Ports Collection] is available and ready to use on the local system.
Please consult the FreeBSD documentation to understand how to install
and use it.  In the rest of this document, the related details will
not be discussed.

Before trying to build the `net/wifibox-alpine` port, it might be
easier if the dependencies are installed beforehand via packages.

```console
# pkg install gtar patchelf squashfs-tools-ng
```

Also, make sure that the [Linuxulator] is activated, for example, by
loading the corresponding kernel module.  There is no need to install
a complete Linux base system (the port itself is going to deploy the
tooling necessary for that), but if that is already there it will not
cause a problem either.

```console
# kldload linux64
```

Sometimes the build may crash with an `Abort trap` message, which
results in `Error 134` for `make`.  This happens when trying to run
Linux binaries atop FreeBSD, such as the APK package manager for
installing the components.  Systems that have not had the Linuxulator
used before and may not have the `/compat/linux` directory created,
which is expected to be present.  This issue can be fixed as follows,
for example.

```console
# mkdir -p /compat/linux
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
made, one might want to check and change the configuration options
available before installing, reinstalling, or updating the ports.

```console
# make -C net/wifibox-alpine config
# make -C net/wifibox-core config
```

Note that some of the options might make sense only by studying the
[`freebsd-wifibox`] and [`freebsd-wifibox-alpine`] repositories.
Other than that, the ports can be easily installed in the standard
way.

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

[FreeBSD Ports Collection]: https://docs.freebsd.org/en/books/handbook/ports/#ports-using
[Linuxulator]: https://docs.freebsd.org/en/books/handbook/linuxemu/
[Wifibox project]: https://github.com/pgj/freebsd-wifibox
[`freebsd-wifibox`]: https://github.com/pgj/freebsd-wifibox
[`freebsd-wifibox-alpine`]: https://github.com/pgj/freebsd-wifibox-alpine
