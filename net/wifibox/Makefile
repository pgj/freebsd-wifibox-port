PORTNAME=	wifibox
PORTVERSION=	0.1.0
CATEGORIES=	net

MAINTAINER=	pali.gabor@gmail.com
COMMENT=	Wireless card driver via virtualized Linux

LICENSE=	BSD2CLAUSE
LICENSE_FILE=	${WRKSRC}/LICENSE

ONLY_FOR_ARCHS=	amd64
EXTRACT_ONLY=	${GH_ACCOUNT}-${GH_PROJECT}-${GH_TAGNAME_EXTRACT}_GH0.tar.gz

RUN_DEPENDS=	grub2-bhyve>0:sysutils/grub2-bhyve

USE_GITHUB=	yes
GH_ACCOUNT=	pgj
GH_PROJECT=	freebsd-wifibox

MASTER_SITES+=	https://github.com/pgj/freebsd-wifibox-image/releases/download/snapshot-20210718/
DISTFILES+=	freebsd-wifibox-appliance.20210718.img.xz

NO_BUILD=	yes
MAKE_ENV+=	IMGXZ=${DISTDIR}/freebsd-wifibox-appliance.20210718.img.xz

.include <bsd.port.mk>
