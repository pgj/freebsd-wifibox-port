PORTNAME=	wifibox-alpine
PORTVERSION=	20220411
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
OPTIONS_MULTI_FIRMWARE=	FW_ATH6K FW_ATH9K FW_ATH10K FW_ATH11K FW_B43 FW_B43LEGACY\
			FW_BRCM FW_IWLWIFI FW_MWLWIFI FW_RTLWIFI FW_RTW88 FW_RTW89

OPTIONS_DEFAULT+=	FW_ATH6K FW_ATH9K FW_ATH10K FW_ATH11K FW_BRCM FW_IWLWIFI\
			FW_MWLWIFI FW_RTLWIFI FW_RTW88 FW_RTW89

FW_ATH6K_DESC=		ath6k
FW_ATH9K_DESC=		ath9k_htc
FW_ATH10K_DESC=		ath10k
FW_ATH11K_DESC=		ath11k
FW_BRCM_DESC=		brcm
FW_IWLWIFI_DESC=	iwlwifi
FW_MWLWIFI_DESC=	mwlwifi
FW_RTLWIFI_DESC=	rtlwifi
FW_RTW88_DESC=		rtw88
FW_RTW89_DESC=		rtw89
FW_B43LEGACY_DESC=	b43legacy
FW_B43_DESC=		b43
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

_LINUXFW_SITE=	https://git.kernel.org/pub/scm/linux/kernel/git/firmware/
_LINUXFW_TAG=	20220411
_LINUXFW=	linux-firmware-${_LINUXFW_TAG}

MASTER_SITES+=	${_ALPINE_SITE}/releases/x86_64/:rootfs \
		${_ALPINE_SITE}/main/x86_64/:main_packages \
		${_ALPINE_SITE}/community/x86_64/:community_packages \
		${_GITHUB_SITE}/${_GITHUB_TAG}/:custom_packages \
		${_LINUXFW_SITE}/linux-firmware.git/snapshot/:linux_firmware \
		https://sources.openwrt.org/:openwrt \
		https://lwfinger.com/b43-firmware/:lwfinger

_MINIROOTFS=	alpine-minirootfs-${ALPINE_VERSION}-x86_64.tar.gz
_PACKAGES=	baselayout-3.2.0-r18:wifibox \
		busybox-1.34.1-r5:wifibox \
		ifupdown-ng-0.11.3-r0:wifibox \
		iptables-1.8.7-r1:wifibox \
		iw-5.16-r0:community \
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

DIST_SUBDIR=	wifibox-alpine
DISTFILES+=	${_MINIROOTFS}:rootfs

.if ${PORT_OPTIONS:MFW_*:NFW_B43*}
_LINUXFWFILE=	${_LINUXFW}.tar.gz
_LINUXFWDIR=	${WRKDIR}/${_LINUXFW}
DISTFILES+=	${_LINUXFWFILE}:linux_firmware
.endif

.for package in ${_PACKAGES}
.if ${package:M*\:wifibox}
DISTFILES+=	${package:C@^([^:]*):wifibox$@\1@}.apk:custom_packages
.elif ${package:M*\:community}
DISTFILES+=	${package:C@^([^:]*):community$@\1@}.apk:community_packages
.else
DISTFILES+=	${package:C@^([^:]*):wifibox$@\1@}.apk:main_packages
.endif
_APK_FILES+=	${_DISTDIR}/${package:C@^([^:]*):.*$@\1@}.apk
.endfor

_FIRMWAREDIR=	${WRKSRC}/guest/lib/firmware

.if ${PORT_OPTIONS:MFW_B43LEGACY}
DISTFILES+=	${_B43LEGACY_FIRMWARE}.o:openwrt
.endif

.if ${PORT_OPTIONS:MFW_B43}
DISTFILES+=	${_B43_FIRMWARE}.tar.bz2:lwfinger
EXTRACT_ONLY+=	${_B43_FIRMWARE}.tar.bz2
.endif

.if ${PORT_OPTIONS:MFW_B43*}
BUILD_DEPENDS+=		b43-fwcutter>0:sysutils/b43-fwcutter
FW_B43_TARGETDIR=	${_FIRMWAREDIR}/b43
FW_B43_FIRMWARE=	${WRKDIR}/${_B43_FIRMWARE}.wl_apsta.o
FW_B43LEGACY_TARGETDIR=	${_FIRMWAREDIR}/b43legacy
FW_B43LEGACY_FIRMWARE=	${_DISTDIR}/${_B43LEGACY_FIRMWARE}.o
.endif

FW_ATH6K_FILES=		ath6k
FW_ATH9K_FILES=		ath9k_htc
FW_ATH10K_FILES=	ath10k
FW_ATH11K_FILES=	ath11k
FW_BRCM_FILES=		brcm
FW_IWLWIFI_FILES=	iwlwifi-*
FW_MWLWIFI_FILES=	mwlwifi
FW_RTLWIFI_FILES=	rtlwifi
FW_RTW88_FILES=		rtw88
FW_RTW89_FILES=		rtw89

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
.if defined(_LINUXFWDIR)
	${TAR} -xf ${_DISTDIR}/${_LINUXFWFILE} -C ${WRKDIR}
	${MKDIR} ${_FIRMWAREDIR}
.endif
.for fw_opt in ${PORT_OPTIONS:MFW_*:NFW_B43*}
.if defined(${fw_opt}_FILES)
	${CP} -R ${_LINUXFWDIR}/${${fw_opt}_FILES} ${_FIRMWAREDIR}
.endif
.endfor

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
