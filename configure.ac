AC_PREREQ(2.60)
AC_INIT(knot, 0.01)

AM_INIT_AUTOMAKE([silent-rules subdir-objects])

AC_CONFIG_HEADERS(config.h)

m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

AM_MAINTAINER_MODE

AC_PREFIX_DEFAULT(/usr/local)

PKG_PROG_PKG_CONFIG

COMPILER_WARNING_CFLAGS
COMPILER_BUILD_CFLAGS

AC_LANG_C

AC_PROG_CC
AC_PROG_CC_PIE

m4_define([_LT_AC_TAGCONFIG], [])
m4_ifdef([AC_LIBTOOL_TAGS], [AC_LIBTOOL_TAGS([])])

AC_DISABLE_STATIC
AC_PROG_LIBTOOL

PKG_CHECK_MODULES(GLIB, glib-2.0 >= 2.28, dummy=no,
				AC_MSG_ERROR(required glib >= 2.28))
AC_SUBST(GLIB_CFLAGS)
AC_SUBST(GLIB_LIBS)

PKG_CHECK_MODULES(JSON, json-c,
  [AC_DEFINE([HAVE_JSON],[1],[Use JSON])],
  [AC_MSG_ERROR("json-c missing")])
AC_SUBST(JSON_CFLAGS)
AC_SUBST(JSON_LIBS)

PKG_CHECK_MODULES(ELL, ell,
  [AC_DEFINE([HAVE_ELL],[1],[Use ELL])],
  [AC_MSG_ERROR("ell missing")])
AC_SUBST(ELL_CFLAGS)
AC_SUBST(ELL_LIBS)

PKG_CHECK_MODULES(KNOTPROTO, knotprotocol,
  [AC_DEFINE([HAVE_KNOTPROTO],[1],[Use KNOTPROTO])],
  [AC_MSG_ERROR("knotprotocol missing")])
AC_SUBST(KNOTPROTO_CFLAGS)
AC_SUBST(KNOTPROTO_LIBS)

PKG_CHECK_MODULES(KNOTHAL, hal,
  [AC_DEFINE([HAVE_KNOTHAL],[1],[Use KNOTHAL])],
  [AC_MSG_ERROR("KNoT-HAL missing")])
AC_SUBST(KNOTHAL_CFLAGS)
AC_SUBST(KNOTHAL_LIBS)

PKG_CHECK_MODULES(WEBSOCKETS, [libwebsockets >= 2.4.2],
  [AC_DEFINE([HAVE_WEBSOCKETS],[1],[Use WEBSOCKETS])],
  [AC_MSG_ERROR("libwebsockets missing")])
AC_SUBST(WEBSOCKETS_CFLAGS)
AC_SUBST(WEBSOCKETS_LIBS)

AM_CONDITIONAL(WEBSOCKETS, (test "${websockets}" != "no"))

AC_ARG_WITH([dbusconfdir], AC_HELP_STRING([--with-dbusconfdir=DIR],
				[path to D-Bus configuration directory]),
					[path_dbusconfdir=${withval}])

if (test -z "${path_dbusconfdir}"); then
	AC_MSG_CHECKING([D-Bus config directory])
	path_dbusconfdir="`$PKG_CONFIG --variable=sysconfdir dbus-1`"
	if (test -z "${path_dbusconfdir}"); then
		AC_MSG_ERROR([D-Bus config directory is required])
	fi
	AC_MSG_RESULT([${path_dbusconfdir}])
fi

AC_SUBST(DBUS_CONFDIR, [${path_dbusconfdir}])

if (test "$sysconfdir" = '${prefix}/etc'); then
	knotconfigdir="${prefix}/etc/knot"
else
	knotconfigdir="${sysconfdir}/knot"
fi

AC_DEFINE_UNQUOTED(KNOTCONFIGDIR, "${knotconfigdir}",
			[Directory for the KNoT configuration files])
AC_SUBST(KNOTCONFIGDIR, "${knotconfigdir}")

AC_OUTPUT(Makefile)
