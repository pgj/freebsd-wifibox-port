PORTNAME=	wifibox
PORTVERSION=	0.6.0
CATEGORIES=	net

MAINTAINER=	pali.gabor@gmail.com
COMMENT=	Wireless card driver via virtualized Linux

LICENSE=	BSD2CLAUSE
LICENSE_FILE=	${WRKSRC}/LICENSE

ONLY_FOR_ARCHS=	amd64
EXTRACT_ONLY=	${GH_ACCOUNT}-${GH_PROJECT}-${PORTVERSION}-${GH_TAGNAME_EXTRACT}_GH0.tar.gz

RUN_DEPENDS=	grub2-bhyve>0:sysutils/grub2-bhyve \
		bhyve+>0:sysutils/bhyve+

USE_GITHUB=	yes
GH_ACCOUNT=	pgj
GH_PROJECT=	freebsd-wifibox
GH_TAGNAME=	a2ada9225ecf735e48a1304fcae7c384e1335c22

IMAGE_VERSION=	snapshot-20210905
IMAGE_FILE=	freebsd-wifibox-appliance.${IMAGE_VERSION}.img.xz

MASTER_SITES+=	https://github.com/pgj/freebsd-wifibox-image/releases/download/${IMAGE_VERSION}/
DISTFILES+=	${IMAGE_FILE}

NO_BUILD=	yes
MAKE_ENV+=	IMGXZ=${DISTDIR}/${IMAGE_FILE} \
		VERSION=${PORTVERSION} \
		BHYVE=${LOCALBASE}/sbin/bhyve \
		BHYVECTL=${LOCALBASE}/sbin/bhyvectl \
		VMM_KO=${KMODDIR}/vmm.ko

.include <bsd.port.mk>
