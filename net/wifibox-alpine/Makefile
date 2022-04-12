PORTNAME=	wifibox-alpine
PORTVERSION=	${PACKAGE_SET}
CATEGORIES=	net

MAINTAINER=	pali.gabor@gmail.com
COMMENT=	Wifibox guest based on Alpine Linux

LICENSE=	BSD2CLAUSE
LICENSE_FILE=	${WRKSRC_scripts}/LICENSE

ONLY_FOR_ARCHS=	amd64
EXTRACT_ONLY=	${_DISTFILES:M*_GH0.tar.gz}

BUILD_DEPENDS=	patchelf>0:sysutils/patchelf \
		squashfs-tools>0:sysutils/squashfs-tools

.include "${.CURDIR}/flavors.mk"

.for flavor in ${_FLAVORS}
${flavor}_PKGNAMESUFFIX=	-${flavor}
.endfor

OPTIONS_SUB=			yes

OPTIONS_SINGLE=			COMPRESSION
OPTIONS_SINGLE_COMPRESSION=	COMP_GZIP COMP_LZO COMP_LZ4 COMP_XZ COMP_ZSTD

OPTIONS_DEFAULT=		COMP_XZ

COMP_GZIP_DESC=		gzip
COMP_LZO_DESC=		lzo
COMP_LZ4_DESC=		lz4
COMP_XZ_DESC=		xz
COMP_ZSTD_DESC=		zstd

.if ${FLAVOR:U} == default
OPTIONS_MULTI=		FIRMWARE
OPTIONS_MULTI_FIRMWARE=	FW_ATH6K FW_ATH9K FW_ATH10K FW_ATH11K FW_BRCM FW_MWLWIFI \
			FW_OTHER FW_RTLWIFI FW_RTW88 FW_RTW89 FW_B43LEGACY FW_B43

OPTIONS_DEFAULT+=	FW_ATH6K FW_ATH9K FW_ATH10K FW_ATH11K FW_BRCM FW_MWLWIFI \
			FW_OTHER FW_RTLWIFI FW_RTW88 FW_RTW89

FW_ATH6K_DESC=		linux-firmware-ath6k
FW_ATH9K_DESC=		linux-firmware-ath9k_htc
FW_ATH10K_DESC=		linux-firmware-ath10k
FW_ATH11K_DESC=		linux-firmware-ath11k
FW_BRCM_DESC=		linux-firmware-brcm
FW_MWLWIFI_DESC=	linux-firmware-mwlwifi
FW_OTHER_DESC=		linux-firmware-other (iwlwifi, ath3k, ar[579]xxx)
FW_RTLWIFI_DESC=	linux-firmware-rtlwifi
FW_RTW88_DESC=		linux-firmware-rtw88
FW_RTW89_DESC=		linux-firmware-rtw89
FW_B43LEGACY_DESC=	b43legacy-firmware
FW_B43_DESC=		b43-firmware
.endif

.for flavor in ${_FLAVORS}
.if ${FLAVOR:U} == ${flavor}
PORT_OPTIONS+=		FW_${flavor:tu}
.endif
.endfor

PACKAGE_SET=	20220410
_GITHUB_TAG=	snapshot-${PACKAGE_SET}
_GITHUB_SITE=	https://github.com/pgj/freebsd-wifibox-alpine/releases/download

USE_GITHUB=	nodefault
GH_ACCOUNT=	pgj
GH_PROJECT=	freebsd-wifibox-alpine:scripts
GH_TAGNAME=	${_GITHUB_TAG}:scripts

ALPINE_VERSION=	3.15.4
_ALPINE_VER=	${ALPINE_VERSION:C@([0-9]+)\.([0-9]+).*@\1.\2@}

.if defined(ALPINE_DATE)
_SNAPSHOT=	${ALPINE_DATE:S/./-/g}
_ALPINE_SITE=	https://mirror.fit.cvut.cz/alpine/snapshots/${_SNAPSHOT}/v${_ALPINE_VER}
PORTVERSION=	${ALPINE_VERSION}.${ALPINE_DATE}
.else
_ALPINE_SITE=	https://mirror.fit.cvut.cz/alpine/v${_ALPINE_VER}
.endif

MASTER_SITES+=	${_ALPINE_SITE}/releases/x86_64/:rootfs \
		${_ALPINE_SITE}/main/x86_64/:packages \
		${_GITHUB_SITE}/${_GITHUB_TAG}/:custom_packages \
		https://sources.openwrt.org/:openwrt \
		https://lwfinger.com/b43-firmware/:lwfinger

_MINIROOTFS=	alpine-minirootfs-${ALPINE_VERSION}-x86_64.tar.gz
_PACKAGES=	baselayout-3.2.0-r18:wifibox \
		busybox-1.34.1-r5:wifibox \
		ifupdown-ng-0.11.3-r0:wifibox \
		iptables-1.8.7-r1:wifibox \
		libcrypto1.1-1.1.1n-r0 \
		libmnl-1.0.4-r2 \
		libnftnl-1.2.1-r0 \
		libnl3-3.5.0-r0 \
		libssl1.1-1.1.1n-r0 \
		linux-lts-5.15.33-r0:wifibox \
		musl-1.2.2-r7 \
		openrc-0.44.7-r5:wifibox \
		pcsc-lite-libs-1.9.4-r0 \
		socat-1.7.4.2-r0:wifibox \
		wpa_passthru-0.1.0-r0:wifibox \
		wpa_supplicant-2.9-r17:wifibox

_FW_PACKAGES=	ATH6K:ath6k-20211027-r0 \
		ATH9K:ath9k_htc-20211027-r0 \
		ATH10K:ath10k-20211027-r0 \
		ATH11K:ath11k-20211027-r0 \
		BRCM:brcm-20211027-r0 \
		MWLWIFI:mwlwifi-20211027-r0 \
		OTHER:other-20211027-r0 \
		RTLWIFI:rtlwifi-20211027-r0 \
		RTW88:rtw88-20211027-r0 \
		RTW89:rtw89-20211027-r0

_B43LEGACY_FIRMWARE=	wl_apsta-3.130.20.0
_B43_FIRMWARE=		broadcom-wl-6.30.163.46

pre-everything::
.if !defined(FIRMWARE_FILES)
	@${ECHO_MSG} "===> NOTICE:"
	@${ECHO_MSG}
	@${ECHO_MSG} "Use the FIRMWARE_FILES make(1) variable to keep only the firmware"
	@${ECHO_MSG} "files that are used by the driver to reduce the size of the disk"
	@${ECHO_MSG} "image."
	@${ECHO_MSG}
	@sleep 1
.endif

.include <bsd.port.options.mk>

.for config in ${_FW_PACKAGES}
.if ${PORT_OPTIONS:MFW_${config:C@^(.*):(.*)$@\1@}}
_PACKAGES+=	linux-firmware-${config:C@^(.*):(.*)$@\2@}
.endif
.endfor

DIST_SUBDIR=	wifibox-alpine
DISTFILES+=	${_MINIROOTFS}:rootfs
.for package in ${_PACKAGES}
.if ${package:M*\:wifibox}
DISTFILES+=	${package:C@^([^:]*):wifibox$@\1@}.apk:custom_packages
.else
DISTFILES+=	${package:C@^([^:]*):wifibox$@\1@}.apk:packages
.endif
_APK_FILES+=	${_DISTDIR}/${package:C@^([^:]*):wifibox$@\1@}.apk
.endfor

.if ${PORT_OPTIONS:MFW_B43LEGACY}
DISTFILES+=	${_B43LEGACY_FIRMWARE}.o:openwrt
.endif

.if ${PORT_OPTIONS:MFW_B43}
DISTFILES+=	${_B43_FIRMWARE}.tar.bz2:lwfinger
EXTRACT_ONLY+=	${_B43_FIRMWARE}.tar.bz2
.endif

.if ${PORT_OPTIONS:MFW_B43*}
BUILD_DEPENDS+=		b43-fwcutter>0:sysutils/b43-fwcutter
_FIRMWAREDIR=		${WRKSRC}/guest/lib/firmware
FW_B43_TARGETDIR=	${_FIRMWAREDIR}/b43
FW_B43_FIRMWARE=	${WRKDIR}/${_B43_FIRMWARE}.wl_apsta.o
FW_B43LEGACY_TARGETDIR=	${_FIRMWAREDIR}/b43legacy
FW_B43LEGACY_FIRMWARE=	${_DISTDIR}/${_B43LEGACY_FIRMWARE}.o

pre-build:
.for fw in ${PORT_OPTIONS:MFW_B43*}
.if ${fw}
	(_TARGETDIR=${${fw}_TARGETDIR}; \
	${MKDIR} $$_TARGETDIR; \
	${LOCALBASE}/bin/b43-fwcutter -w $$_TARGETDIR ${${fw}_FIRMWARE}; \
	${FIND} $$_TARGETDIR -mindepth 2 -type f -exec ${MV} {} $$_TARGETDIR \; ; \
	${FIND} $$_TARGETDIR -type d -delete)
.endif
.endfor
.endif

post-install:
	${PRINTF} "cpus=1\nmemory=45M\n" \
		> ${STAGEDIR}${PREFIX}/share/wifibox/bhyve.conf

WRKSRC=		${WRKSRC_scripts}
MAKE_ENV+=	VERSION=${PORTVERSION} \
		MINIROOTFS=${_DISTDIR}/${_MINIROOTFS} \
		PACKAGES="${_APK_FILES}" \
		SQUASHFS_COMP=${PORT_OPTIONS:MCOMP_*:S/COMP_//:tl}

.if defined(FIRMWARE_FILES)
MAKE_ENV+=	FIRMWARE_FILES=${FIRMWARE_FILES}
.endif

.include <bsd.port.mk>
