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

## Installation

Once this repository is cloned and the necessary preparations are
made, the ports can be easily installed in the standard way.

```console
# make -C net/wifibox-alpine install clean
# make -C net/wifibox install clean
```
