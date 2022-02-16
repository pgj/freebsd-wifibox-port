PORTNAME=	wifibox
PORTVERSION=	1.0.0
CATEGORIES=	net

MAINTAINER=	pali.gabor@gmail.com
COMMENT=	Wireless card driver via virtualized Linux

LICENSE=	BSD2CLAUSE
LICENSE_FILE=	${WRKSRC}/LICENSE

ONLY_FOR_ARCHS=	amd64

RUN_DEPENDS=	grub2-bhyve>0:sysutils/grub2-bhyve

OPTIONS_DEFINE=			BHYVE_PLUS
OPTIONS_EXCLUDE_FreeBSD_12=	BHYVE_PLUS

BHYVE_PLUS_DESC=		Use bhyve+ (experimental)

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MBHYVE_PLUS} || (${OSVERSION} < 1300000)
RUN_DEPENDS+=	bhyve+>0:sysutils/bhyve+
_BHYVE_PLUS=	yes
.endif

USE_GITHUB=	yes
GH_ACCOUNT=	pgj
GH_PROJECT=	freebsd-wifibox freebsd-wifibox-image:image
GH_TAGNAME=	f806bf1866b2f1d837cedef5be4985e0d9faf335
GH_TAGNAME+=	${GUEST_VERSION}:image

GUEST_VERSION= 	snapshot-20220208
GUEST_MAN=	wifibox-alpine.5

# Needed to fake the necessary UID/GID for chrony that runs in the guest
USERS=		squid quagga
GROUPS=		squid quagga

NO_BUILD=	yes
MAKE_ENV+=	GUEST_ROOT=${LOCALBASE}/share/wifibox/guest \
		GUEST_MAN=${WRKSRC_image}/man/${GUEST_MAN} \
		VERSION=${PORTVERSION}
.if defined(_BHYVE_PLUS)
MAKE_ENV+=	BHYVE=${LOCALBASE}/sbin/bhyve \
		BHYVECTL=${LOCALBASE}/sbin/bhyvectl \
		VMM_KO=${KMODDIR}/vmm.ko
.endif

.include <bsd.port.mk>
