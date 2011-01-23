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

SUBDIRS = man commands contexts

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

EXTRA_DIST = rstart.in rstartd.in config.in

MAINTAINERCLEANFILES = ChangeLog INSTALL
CLEANFILES = rstart rstartd config

EXTRA_DIST +=								\
	server.os2							\
									\
	samples/commands/openwindows2/Terminal				\
	samples/commands/openwindows2/LoadMonitor			\
	samples/commands/openwindows2/@List				\
	samples/commands/ListContexts					\
	samples/commands/x11r5/Terminal					\
	samples/commands/x11r5/LoadMonitor				\
	samples/commands/x11r5/@List					\
	samples/commands/odt1/Terminal					\
	samples/commands/odt1/LoadMonitor				\
	samples/commands/odt1/@List					\
	samples/commands/@List						\
	samples/commands/ListGenericCommands				\
	samples/commands/openwindows3/Terminal				\
	samples/commands/openwindows3/LoadMonitor			\
	samples/commands/openwindows3/@List				\
	samples/contexts.odt1/openwindows2				\
	samples/contexts.odt1/default					\
	samples/contexts.odt1/@Aliases					\
	samples/contexts.odt1/x11r5					\
	samples/contexts.odt1/odt1					\
	samples/contexts.odt1/x11r6					\
	samples/contexts.odt1/@List					\
	samples/contexts.odt1/openwindows3				\
									\
	specs/fix.awk							\
	specs/fix.nawk							\
	specs/fix.sed							\
	specs/rstart.ms							\
	specs/rstartd.txt						\
	specs/tmac.rfc



.PHONY: ChangeLog INSTALL

INSTALL:
	$(INSTALL_CMD)

ChangeLog:
	$(CHANGELOG_CMD)

dist-hook: ChangeLog INSTALL
