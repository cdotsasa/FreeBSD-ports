# Created by: Mohammad S. Babaei <info@babaei.net>
# $FreeBSD$

PORTNAME=	wt3
PORTVERSION=	3.6.0
CATEGORIES=	www

MAINTAINER=	info@babaei.net
COMMENT=	Widget-centric C++ library for developing web applications

LICENSE=	GPLv2
LICENSE_FILE=	${WRKSRC}/LICENSE

LIB_DEPENDS=	libpng.so:graphics/png \
		libboost_system.so:devel/boost-libs \
		libGraphicsMagick.so:graphics/GraphicsMagick

USES=		cmake compiler:c++14-lang pkgconfig ssl xorg
USE_GITHUB=	yes
GH_ACCOUNT=	emweb
GH_PROJECT=	wt
USE_XORG=	x11
USE_LDCONFIG=	yes

CMAKE_ARGS=	-DCONFIGDIR:STRING=${LOCALBASE}/etc/${PORTNAME} \
	-DGM_PREFIX:STRING=${LOCALBASE} \
	-DEXAMPLES_DESTINATION:STRING=share/examples/${PORTNAME} \
	-DSSL_PREFIX:STRING=/usr \
	-DZLIB_PREFIX:STRING=/usr \
	-DCONNECTOR_ISAPI:BOOL=OFF \
	-DWEBUSER:STRING=www \
	-DWEBGROUP:STRING=www \
	-DWT_WRASTERIMAGE_IMPLEMENTATION:STRING=GraphicsMagick \
	-DENABLE_SSL:BOOL=ON \
	-DDESTDIR:STRING=${STAGEDIR}

OPTIONS_DEFINE=	\
	EXAMPLES \
	TESTS \
	RESOURCES \
	CMAKE_MODULE \
	CPP11 \
	HARU \
	PANGO \
	OPENGL \
	EXT \
	LIBWTTEST \
	UNWIND
OPTIONS_DEFAULT=	\
	TESTS \
	RESOURCES \
	CMAKE_MODULE \
	CPP11 \
	HARU \
	PANGO \
	OPENGL \
	LIBWTTEST
OPTIONS_GROUP=	CONNECTOR DBO WSTRING
OPTIONS_SUB=	yes
NO_OPTIONS_SORT=	yes

EXAMPLES_DESC=		Install examples (implies SQLITE3 and WTHTTP)
TESTS_DESC=		Build Wt tests
RESOURCES_DESC=		Install resources directory
CMAKE_MODULE_DESC=	Install FindWt.cmake in systemwide cmake dir
CPP11_DESC=		Compile Wt in C++11 mode (Clang or GCC 4.6+)
HARU_DESC=		Enable Haru Free PDF Library
EXT_DESC=		Build Wt Ext library with JavaScript-only widgets
LIBWTTEST_DESC=		Build Wt::Test for automated (integration/unit) tests
UNWIND_DESC=		Build Wt with stacktrace support using libunwind

OPTIONS_GROUP_CONNECTOR=	FCGI WTHTTP
OPTIONS_DEFAULT+=	FCGI WTHTTP
CONNECTOR_DESC=		Connector
FCGI_DESC=		Build FastCGI connector
WTHTTP_DESC=		Build Wt stand-alone httpd connector

OPTIONS_GROUP_DBO=	SQLITE3 POSTGRES FIREBIRD MYSQL
OPTIONS_DEFAULT+=	SQLITE3
DBO_DESC=		Wt::DBO
SQLITE3_DESC=		Build Wt with SQLite 3 support
POSTGRES_DESC=		Build Wt with PostgreSQL support
FIREBIRD_DESC=		Build Wt with Firebird support
MYSQL_DESC=		Build Wt with MariaDB or MySQL support

OPTIONS_GROUP_WSTRING=	NO_STD_LOCALE NO_STD_WSTRING
WSTRING_DESC=		Wt::WString
NO_STD_LOCALE_DESC=	Build for a system without std::locale support
NO_STD_WSTRING_DESC=	Build for a system without std::wstring support

EXAMPLES_IMPLIES=	SQLITE3 WTHTTP
EXAMPLES_CMAKE_BOOL=	BUILD_EXAMPLES INSTALL_EXAMPLES

CMAKE_MODULE_CMAKE_BOOL=	INSTALL_FINDWT_CMAKE_FILE

CPP11_CMAKE_ON=	-DWT_CPP_11_MODE:STRING=-std=c++0x

FCGI_CMAKE_BOOL=	CONNECTOR_FCGI
FCGI_LIB_DEPENDS=	libfcgi.so:www/fcgi \
			libfcgi++.so:www/fcgi
FCGI_CMAKE_ON=		-DFCGI_PREFIX:STRING=${LOCALBASE}

FIREBIRD_CMAKE_BOOL=	ENABLE_FIREBIRD
FIREBIRD_USES=		firebird
FIREBIRD_CMAKE_ON=	-DFIREBIRD_PREFIX:STRING=${LOCALBASE} \
			-DENABLE_LIBWTDBO:BOOL=ON

EXT_CMAKE_BOOL=	ENABLE_EXT

HARU_CMAKE_BOOL=	ENABLE_HARU
HARU_LIB_DEPENDS=	libhpdf.so:print/libharu
HARU_CMAKE_ON=	-DHARU_PREFIX:STRING=${LOCALBASE}

LIBWTTEST_CMAKE_BOOL=	ENABLE_LIBWTTEST

MYSQL_USES=		mysql
MYSQL_CMAKE_BOOL=	ENABLE_MYSQL
MYSQL_CMAKE_ON=		-DMYSQL_PREFIX:STRING=${LOCALBASE} \
			-DENABLE_LIBWTDBO:BOOL=ON

NO_STD_LOCALE_CMAKE_BOOL=	WT_NO_STD_LOCALE

NO_STD_WSTRING_CMAKE_BOOL=	WT_NO_STD_WSTRING

OPENGL_CMAKE_BOOL=	ENABLE_OPENGL
OPENGL_USE=		GL=gl,glew
OPENGL_USES=		gl

PANGO_CMAKE_BOOL=	ENABLE_PANGO
PANGO_USE=		GNOME=pango
PANGO_USES=		gnome
PANGO_LIB_DEPENDS=	libfontconfig.so:x11-fonts/fontconfig

POSTGRES_CMAKE_BOOL=	ENABLE_POSTGRES
POSTGRES_USES=		pgsql
POSTGRES_CMAKE_ON=	-DPOSTGRES_PREFIX:STRING=${LOCALBASE} \
			-DENABLE_LIBWTDBO:BOOL=ON

RESOURCES_CMAKE_BOOL=	INSTALL_RESOURCES

SQLITE3_CMAKE_BOOL=	ENABLE_SQLITE
SQLITE3_USES=		sqlite:3
SQLITE3_CMAKE_ON=	-DENABLE_LIBWTDBO:BOOL=ON

TESTS_CMAKE_BOOL=	BUILD_TESTS

UNWIND_CMAKE_BOOL=	ENABLE_UNWIND
UNWIND_LIB_DEPENDS=	libunwind.so:devel/libunwind

WTHTTP_CMAKE_BOOL=	CONNECTOR_HTTP

PORTEXAMPLES=	*

.include <bsd.port.options.mk>

.if ${PORT_OPTIONS:MSQLITE3} || \
    ${PORT_OPTIONS:MPOSTGRES} || \
    ${PORT_OPTIONS:MFIREBIRD} || \
    ${PORT_OPTIONS:MMYSQL}
PLIST_SUB+=	DBO=""
.else
PLIST_SUB+=	DBO="@comment "
.endif

post-install:
	@${RM} ${STAGEDIR}${ETCDIR}/wt_config.xml
	@${MKDIR} ${STAGEDIR}${ETCDIR}
	${INSTALL_DATA} ${BUILD_WRKSRC}/wt_config.xml \
		${STAGEDIR}${ETCDIR}/wt_config.xml.sample

.include <bsd.port.mk>
