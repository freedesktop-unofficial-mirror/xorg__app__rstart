# 
#  Copyright 2005  Red Hat, Inc.
# 
#  Permission to use, copy, modify, distribute, and sell this software and its
#  documentation for any purpose is hereby granted without fee, provided that
#  the above copyright notice appear in all copies and that both that
#  copyright notice and this permission notice appear in supporting
#  documentation, and that the name of Red Hat not be used in
#  advertising or publicity pertaining to distribution of the software without
#  specific, written prior permission.  Red Hat makes no
#  representations about the suitability of this software for any purpose.  It
#  is provided "as is" without express or implied warranty.
# 
#  RED HAT DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE,
#  INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS, IN NO
#  EVENT SHALL RED HAT BE LIABLE FOR ANY SPECIAL, INDIRECT OR
#  CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE,
#  DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER
#  TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
#  PERFORMANCE OF THIS SOFTWARE.

SUBDIRS = man commands contexts samples specs

# rstartd server

rstart_PROGRAMS = rstartd.real

AM_CFLAGS = $(CWARNFLAGS) $(RSTART_CFLAGS)		\
		-DSERVERNAME=\"rstart\"			\
		-DDEFAULT_CONFIG=\"$(rstartdir)/config\"

rstartd_real_LDADD = $(RSTART_LIBS)

rstartd_real_SOURCES =	\
        auth.c \
        server.c \
        server.h

# config data

rstart_DATA = config

CONFIG_SUBSTS = -e 's|@rstartdir[@]|$(rstartdir)|g'
config: config.in
	$(AM_V_GEN)$(SED) $(CONFIG_SUBSTS) < $< > $@

# wrapper scripts

bin_SCRIPTS = rstart rstartd

RSTART_SUBSTS = -e 's|@RSH[@]|$(RSH)|g'
rstart: rstart.in
	$(AM_V_GEN)$(SED) $(RSTART_SUBSTS) < $< > $@

RSTARTD_SUBSTS = -e 's|@rstartdir[@]|$(rstartdir)|g'
rstartd: rstartd.in
	$(AM_V_GEN)$(SED) $(RSTARTD_SUBSTS) < $< > $@

CLEANFILES = rstart rstartd config
EXTRA_DIST = rstart.in rstartd.in config.in server.os2

MAINTAINERCLEANFILES = ChangeLog INSTALL

.PHONY: ChangeLog INSTALL

INSTALL:
	$(INSTALL_CMD)

ChangeLog:
	$(CHANGELOG_CMD)

dist-hook: ChangeLog INSTALL
