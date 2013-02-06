AC_PREREQ([2.53])

dnl --------------------------------------------------------------
dnl Making releases: dvdread_sub += 1; change DVDREAD_LT_* accordingly
dnl
dnl These are defined in m4 so they can be passed to AC_INIT
dnl --------------------------------------------------------------
m4_define([dvdread_major], [4])
m4_define([dvdread_minor], [2])
m4_define([dvdread_sub],   [0])
m4_define([dvdread_pre],   [])

AC_INIT([libdvdread], [dvdread_major.dvdread_minor.dvdread_sub[]dvdread_pre])
AC_CONFIG_SRCDIR([src/dvdread/dvd_reader.h])
AM_INIT_AUTOMAKE([1.6 foreign])

dnl create a config.h file (Automake will add -DHAVE_CONFIG_H)
AC_CONFIG_HEADERS([config.h])
AC_CANONICAL_HOST

DVDREAD_MAJOR="dvdread_major"
DVDREAD_MINOR="dvdread_minor"
DVDREAD_SUB="dvdread_sub"
DVDREAD_PRE="dvdread_pre"

AC_SUBST(DVDREAD_MAJOR)
AC_SUBST(DVDREAD_MINOR)
AC_SUBST(DVDREAD_SUB)

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

AC_SUBST(DVDREAD_LT_CURRENT)
AC_SUBST(DVDREAD_LT_AGE)
AC_SUBST(DVDREAD_LT_REVISION)

AC_PROG_CC

LT_INIT

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

dnl ---------------------------------------------
dnl Check for doxygen (dynamic documentation generator)
dnl ---------------------------------------------
AC_CHECK_PROG(DOXYGEN, doxygen, doxygen, no)

dnl ---------------------------------------------
dnl Output configuration files
dnl ---------------------------------------------
AC_OUTPUT([
Makefile
misc/dvdread.pc
misc/libdvdread.spec
])
