AM_CPPFLAGS = -I$(top_srcdir)/src $(CSS_CFLAGS)

EXTRA_DIST = \
	README \
	TODO \
	msvc/

lib_LTLIBRARIES = libdvdread.la

libdvdread_la_SOURCES = \
	src/bitreader.c \
	src/bswap.h \
	src/dvd_input.c \
	src/dvd_input.h \
	src/dvd_reader.c \
	src/dvd_udf.c \
	src/dvdread_internal.h \
	src/ifo_print.c \
	src/ifo_read.c \
	src/md5.c \
	src/md5.h \
	src/nav_print.c \
	src/nav_read.c

libdvdread_la_LIBADD = $(CSS_LIBS)

libdvdread_la_LDFLAGS = -version-info $(DVDREAD_LT_CURRENT):$(DVDREAD_LT_REVISION):$(DVDREAD_LT_AGE) \
	-export-symbols-regex "(^dvd.*|^nav.*|^ifo.*|^DVD.*|^UDF.*)"

pkgincludedir = $(includedir)/dvdread
pkginclude_HEADERS = \
	src/dvdread/bitreader.h \
	src/dvdread/dvd_reader.h \
	src/dvdread/dvd_udf.h \
	src/dvdread/ifo_print.h \
	src/dvdread/ifo_read.h \
	src/dvdread/ifo_types.h \
	src/dvdread/nav_print.h \
	src/dvdread/nav_read.h \
	src/dvdread/nav_types.h

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = misc/dvdread.pc
