AC_PREREQ([2.53])

AC_INIT([dvd5], [5])
AC_CONFIG_SRCDIR([src/ifo_read.c])
AM_INIT_AUTOMAKE([1.6 foreign])

dnl create a config.h file (Automake will add -DHAVE_CONFIG_H)
AC_CONFIG_HEADERS([config.h])
AC_CANONICAL_HOST

dnl The libtool version numbers (DVD5_LT_*); Don't even think about faking this!
dnl
dnl immediately before every release do:
dnl ===================================
dnl if (the interface is totally unchanged from previous release)
dnl    DVD5_LT_REVISION ++;
dnl else { /* interfaces have been added, removed or changed */
dnl    DVD5_LT_REVISION = 0;
dnl    DVD5_LT_CURRENT ++;
dnl    if (any interfaces have been _added_ since last release)
dnl       AGE ++;
dnl    if (any interfaces have been _removed_ or _incompatibly changed_)
dnl       AGE = 0;
dnl }
dnl
dnl If you want to know more about what you are doing, here are some details:
dnl  * DVD5_LT_CURRENT is the current API version
dnl  * DVD5_LT_REVISION is an internal revision number which is increased when the API
dnl    itself did not change
dnl  * DVD5_LT_AGE is the number of previous API versions still supported by this library
dnl  * libtool has its own numbering scheme, because local library numbering schemes
dnl    are platform dependent
dnl  * in Linux, the library will be named
dnl    libname.so.(DVD5_LT_CURRENT - DVD5_LT_AGE).DVD5_LT_AGE.DVD5_LT_REVISION

DVD5_LT_CURRENT=0
DVD5_LT_AGE=0
DVD5_LT_REVISION=0

AC_SUBST(DVD5_LT_CURRENT)
AC_SUBST(DVD5_LT_AGE)
AC_SUBST(DVD5_LT_REVISION)

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
dnl threads
dnl ---------------------------------------------
case $host in
  *-*-freebsd*)
    THREAD_LIBS="-L/usr/local/lib -pthread"
    THREAD_CFLAGS="-I/usr/local/include -D_THREAD_SAFE"
    CFLAGS="$THREAD_CFLAGS $CFLAGS"
    ;;
  *mingw32* | *cygwin*)
    ;;
  *)
    AC_CHECK_LIB(pthread, pthread_create,
	[THREAD_LIBS="-lpthread"],
	AC_MSG_ERROR(pthread needed))
    ;;
esac
AC_SUBST(THREAD_LIBS)
AC_SUBST(THREAD_CFLAGS)

dnl ---------------------------------------------
dnl Check for doxygen (dynamic documentation generator)
dnl ---------------------------------------------
AC_CHECK_PROG(DOXYGEN, doxygen, doxygen, no)

dnl ---------------------------------------------
dnl Output configuration files
dnl ---------------------------------------------
AC_OUTPUT([
Makefile
misc/dvd5.pc
])
