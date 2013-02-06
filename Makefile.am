AM_CPPFLAGS = -I$(top_srcdir) -I$(top_srcdir)/src $(THREAD_CFLAGS) $(CSS_CFLAGS)

dist_doc_DATA = AUTHORS ChangeLog COPYING README TODO

EXTRA_DIST = msvc/

lib_LTLIBRARIES = libdvd5.la

libdvd5_la_SOURCES = \
	src/bitreader.c \
	src/bswap.h \
	src/dvd_input.c \
	src/dvd_input.h \
	src/dvd_reader.c \
	src/dvd_udf.c \
	src/dvdnav.c \
	src/dvdnav_internal.h \
	src/dvdread_internal.h \
	src/highlight.c \
	src/ifo_print.c \
	src/ifo_read.c \
	src/md5.c \
	src/md5.h \
	src/nav_print.c \
	src/nav_read.c \
	src/navigation.c \
	src/read_cache.c \
	src/read_cache.h \
	src/remap.c \
	src/remap.h \
	src/searching.c \
	src/settings.c \
	src/vm/decoder.c \
	src/vm/decoder.h \
	src/vm/vm.c \
	src/vm/vm.h

if DEBUG
libdvd5_la_SOURCES += src/vm/trace.c src/vm/trace.h
endif

libdvd5_la_LIBADD = $(CSS_LIBS)

libdvd5_la_LDFLAGS = -version-info $(DVD5_LT_CURRENT):$(DVD5_LT_REVISION):$(DVD5_LT_AGE) \
	-export-symbols-regex "^(dvd|nav|ifo|DVD|UDF)"

pkginclude_HEADERS = \
	dvd5/bitreader.h \
	dvd5/dvd_reader.h \
	dvd5/dvd_types.h \
	dvd5/dvd_udf.h \
	dvd5/dvdnav.h \
	dvd5/dvdnav_events.h \
	dvd5/ifo_print.h \
	dvd5/ifo_read.h \
	dvd5/ifo_types.h \
	dvd5/nav_print.h \
	dvd5/nav_read.h \
	dvd5/nav_types.h

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = misc/dvd5.pc

