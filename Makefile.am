ACLOCAL_AMFLAGS = -I m4

MAINTAINERCLEANFILES = \
Makefile.in \
aclocal.m4 \
compile \
config.guess \
config.sub \
configure \
depcomp \
install-sh \
missing

CLEANFILES = src/logo.h data/logo.ppm

bin_PROGRAMS = src/dietsplash

if CUSTOM_LOGO
logo = @LOGO_PATH@
else
logo = $(abs_top_srcdir)/data/default_logo.ppm
endif

if MAINTAINER_MODE
build_datadir = $(abs_top_builddir)/data
build_bindir = $(abs_top_builddir)/bin
else
build_datadir = $(data_dietsplashdir)
build_bindir = $(bindir)
endif

AM_CFLAGS = -DDATADIR=\""$(build_datadir)"\"
	    -DDIETSPLASH_BIN=\""$(build_bindir)/dietsplash"\"
AM_CPPFLAGS = -I$(builddir)/src -I$(srcdir)/src

src_dietsplash_SOURCES = \
			 src/dietsplash.c \
			 src/fb.c \
			 src/fb.h \
			 src/log.c \
			 src/log.h \
			 src/pnmtologo.c \
			 src/pnmtologo.h \
			 src/util.c \
			 src/util.h

if !STATICLOGO
data_dietsplashdir = $(datadir)/@PACKAGE@
data_dietsplash_DATA = $(logo)
AM_CFLAGS += -DLOGOFILE=\""$(build_datadir)/logo.ppm"\"

if !MAINTAINER_MODE
install-data-hook:
	( cd $(DESTDIR)$(data_dietsplashdir) && \
		rm -f logo.ppm && \
		$(LN_S) $(shell basename ${logo}) logo.ppm )
else
src/dietsplash.o: makelogo

makelogo: $(logo)
	$(AM_V_GEN) \
		( cd $(build_datadir) && rm -f logo.ppm && \
		$(LN_S) $(logo) logo.ppm )
endif
else
noinst_PROGRAMS = src/genstaticlogo

src_genstaticlogo_SOURCES = \
			    src/genstaticlogo.c \
			    src/pnmtologo.c \
			    src/pnmtologo.h \
			    src/util.c \
			    src/util.h

src/logo.h: $(logo) src/genstaticlogo
	$(AM_V_GEN)src/genstaticlogo $< $@

src/dietsplash.o: src/logo.h
endif

EXTRA_DIST = $(srcdir)/data/default_logo.ppm
