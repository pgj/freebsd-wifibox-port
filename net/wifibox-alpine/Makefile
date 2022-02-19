PORTNAME=	wifibox-alpine
PORTVERSION=	${ALPINE_VERSION}.${ALPINE_DATE}
CATEGORIES=	net

MAINTAINER=	pali.gabor@gmail.com
COMMENT=	Wifibox guest based on Alpine Linux

LICENSE=	BSD2CLAUSE
LICENSE_FILE=	${WRKSRC_scripts}/LICENSE

ONLY_FOR_ARCHS=	amd64
EXTRACT_ONLY=	${_DISTFILES:M*_GH0.tar.gz}

USE_GITHUB=	nodefault
GH_ACCOUNT=	pgj
GH_PROJECT=	freebsd-wifibox-alpine:scripts
GH_TAGNAME=	e89c68a5ec81b30542afe1ae0da5fcebc06060c2:scripts

ALPINE_VERSION=	3.15
ALPINE_DATE=	2022.02.05

_SNAPSHOT=	 ${ALPINE_DATE:S/./-/g}
_ALPINE_SITE=	https://mirror.fit.cvut.cz/alpine/snapshots/${_SNAPSHOT}/v${ALPINE_VERSION}

MASTER_SITES+=	${_ALPINE_SITE}/releases/x86_64/:rootfs \
		${_ALPINE_SITE}/main/x86_64/:packages

_MINIROOTFS=	alpine-minirootfs-3.15.0-x86_64.tar.gz
_PACKAGES=	alpine-base-3.15.0-r0 \
		alpine-baselayout-3.2.0-r18 \
		alpine-conf-3.13.0-r0 \
		alpine-keys-2.4-r1 \
		apk-tools-2.12.7-r3 \
		argon2-libs-20190702-r1 \
		busybox-1.34.1-r4 \
		busybox-extras-1.34.1-r4 \
		busybox-initscripts-4.0-r5 \
		busybox-suid-1.34.1-r4 \
		ca-certificates-bundle-20211220-r0 \
		chrony-4.1-r0 \
		chrony-openrc-4.1-r0 \
		cryptsetup-libs-2.4.3-r0 \
		dbus-libs-1.12.20-r4 \
		device-mapper-libs-2.02.187-r2 \
		gmp-6.2.1-r1 \
		gnutls-3.7.1-r0 \
		ifupdown-ng-0.11.3-r0 \
		iptables-1.8.7-r1 \
		iptables-openrc-1.8.7-r1 \
		json-c-0.15-r1 \
		kmod-29-r2 \
		kmod-libs-29-r2 \
		lddtree-1.26-r2 \
		libblkid-2.37.3-r0 \
		libc-utils-0.7.2-r3 \
		libcap-2.61-r0 \
		libcrypto1.1-1.1.1l-r8 \
		libffi-3.4.2-r1 \
		libmnl-1.0.4-r2 \
		libnftnl-1.2.1-r0 \
		libnl3-3.5.0-r0 \
		libretls-3.3.4-r2 \
		libssl1.1-1.1.1l-r8 \
		libtasn1-4.18.0-r0 \
		libunistring-0.9.10-r1 \
		libuuid-2.37.3-r0 \
		linux-firmware-none-20211027-r0 \
		linux-firmware-ath6k-20211027-r0 \
		linux-firmware-ath9k_htc-20211027-r0 \
		linux-firmware-ath10k-20211027-r0 \
		linux-firmware-ath11k-20211027-r0 \
		linux-firmware-brcm-20211027-r0 \
		linux-firmware-mwlwifi-20211027-r0 \
		linux-firmware-other-20211027-r0 \
		linux-firmware-rtlwifi-20211027-r0 \
		linux-firmware-rtw88-20211027-r0 \
		linux-firmware-rtw89-20211027-r0 \
		linux-lts-5.15.16-r0 \
		mkinitfs-3.6.0-r0 \
		musl-1.2.2-r7 \
		musl-utils-1.2.2-r7 \
		nettle-3.7.3-r0 \
		openrc-0.44.7-r3 \
		p11-kit-0.24.0-r1 \
		pcsc-lite-libs-1.9.4-r0 \
		scanelf-1.3.3-r0 \
		ssl_client-1.34.1-r4 \
		wpa_supplicant-2.9-r17 \
		wpa_supplicant-openrc-2.9-r17 \
		xz-libs-5.2.5-r0 \
		zlib-1.2.11-r3 \
		zstd-libs-1.5.0-r0

DIST_SUBDIR=	wifibox-alpine
DISTFILES+=	${_MINIROOTFS}:rootfs
.for package in ${_PACKAGES}
DISTFILES+=	${package}.apk:packages
_APK_FILES+=	${_DISTDIR}/${package}.apk
.endfor

# QA checks are disabled on purpose due to the excessive amount of
# false positive findings.
stage-qa:

# Needed to fake the necessary UID/GID for chrony that runs in the
# guest.
USERS=		squid quagga
GROUPS=		squid quagga

WRKSRC=		${WRKSRC_scripts}
NO_BUILD=	yes
MAKE_ENV+=	VERSION=${PORTVERSION} \
		MINIROOTFS=${_DISTDIR}/${_MINIROOTFS} \
		PACKAGES="${_APK_FILES}"

.include <bsd.port.mk>
