AC_INIT(libdvdread, 4.2.0)
AC_CONFIG_SRCDIR([src/dvd_reader.c])

AC_PREREQ(2.53)
AC_CANONICAL_HOST

AM_INIT_AUTOMAKE([foreign dist-bzip2 no-dist-gzip subdir-objects])
AM_MAINTAINER_MODE([enable])
dnl Enable silent rules only when available (automake 1.11 or later).
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

LT_INIT

AC_CONFIG_HEADER(config.h)
AC_CONFIG_MACRO_DIR([m4])

dnl The libtool version numbers (DVDREAD_LT_*); Don't even think about faking this!
dnl
dnl immediately before every release do:
dnl ===================================
dnl if (the interface is totally unchanged from previous release)
dnl    DVDREAD_LT_REVISION ++;
dnl else { /* interfaces have been added, removed or changed */
dnl    DVDREAD_LT_REVISION = 0;
dnl    DVDREAD_LT_CURRENT ++;
dnl    if (any interfaces have been _added_ since last release)
dnl       AGE ++;
dnl    if (any interfaces have been _removed_ or _incompatibly changed_)
dnl       AGE = 0;
dnl }
dnl
dnl If you want to know more about what you are doing, here are some details:
dnl  * DVDREAD_LT_CURRENT is the current API version
dnl  * DVDREAD_LT_REVISION is an internal revision number which is increased when the API
dnl    itself did not change
dnl  * DVDREAD_LT_AGE is the number of previous API versions still supported by this library
dnl  * libtool has its own numbering scheme, because local library numbering schemes
dnl    are platform dependent
dnl  * in Linux, the library will be named
dnl    libname.so.(DVDREAD_LT_CURRENT - DVDREAD_LT_AGE).DVDREAD_LT_AGE.DVDREAD_LT_REVISION

DVDREAD_LT_CURRENT=5
DVDREAD_LT_AGE=1
DVDREAD_LT_REVISION=2

AC_SUBST([DVDREAD_LTVERSION], [$DVDREAD_LT_CURRENT:$DVDREAD_LT_AGE:$DVDREAD_LT_REVISION])

AC_PROG_CC

AC_CHECK_HEADERS_ONCE([sys/param.h limits.h dlfcn.h])

AC_SYS_LARGEFILE
AC_C_BIGENDIAN

AS_CASE([$host],
  [*mingw32* | *cygwin*], [AC_CHECK_FUNCS(gettimeofday)])

AC_ARG_WITH([libdvdcss],
  AS_HELP_STRING([--with-libdvdcss], [Link directly against libdvdcss @<:@default=no@:>@]))

AC_ARG_ENABLE([dlfcn],
  [AS_HELP_STRING([--enable-dlfcn],
  [use builtin dlfcn for mingw (default is auto)])],
  [use_builtin_dlfcn=$enableval],
  [use_builtin_dlfcn=no])

AS_IF([test x"$with_libdvdcss" = "xyes"], [
  CSS_REQUIRES="libdvdcss >= 1.2"
  PKG_CHECK_MODULES([CSS], [$CSS_REQUIRES])
], [
  AS_CASE([$host],
    [*mingw32*], [],
    [use_builtin_dlfcn=no])

  AS_IF([test $use_builtin_dlfcn = "yes"], [
    AC_DEFINE([USING_BUILTIN_DLFCN], [1], ["Define to 1 to use builtin dlfcn"])
  ], [
    AC_SEARCH_LIBS([dlopen], [dl])
  ])
])
AC_SUBST([CSS_REQUIRES])

CC_CHECK_CFLAGS_APPEND([-Wall -Wsign-compare])


AC_ARG_ENABLE([doc],
  AS_HELP_STRING([--disable-apidoc], [Disable building (with Doxygen) and intalling API documentation @<:@default=auto@:@>]))

AC_PATH_PROG([DOXYGEN], [doxygen])

AS_IF([test "x$DOXYGEN" = "x"], [
  AS_IF([test "x$enable_apidoc" = "xyes"], [
    AC_MSG_ERROR([You need Doxygen to build API documentation])
  ])
])
AM_CONDITIONAL([APIDOC], [test "x$DOXYGEN" != "x"])

dnl ---------------------------------------------
dnl Output configuration files
dnl ---------------------------------------------
AC_OUTPUT([
Makefile
doc/doxygen.cfg
misc/dvdread.pc
misc/libdvdread.spec
])
