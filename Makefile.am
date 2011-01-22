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

SUBDIRS = man

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

# Translate XCOMM into pound sign with sed, rather than passing -DXCOMM=XCOMM
# to cpp, because that trick does not work on all ANSI C preprocessors.
# Delete line numbers from the cpp output (-P is not portable, I guess).
# Allow XCOMM to be preceded by whitespace and provide a means of generating
# output lines with trailing backslashes.
# Allow XHASH to always be substituted, even in cases where XCOMM isn't.

CPP_SED_MAGIC = $(SED) -e '/^\#  *[0-9][0-9]*  *.*$$/d' \
                       -e '/^\#line  *[0-9][0-9]*  *.*$$/d' \
                       -e '/^[         ]*XCOMM$$/s/XCOMM/\#/' \
                       -e '/^[         ]*XCOMM[^a-zA-Z0-9_]/s/XCOMM/\#/' \
                       -e '/^[         ]*XHASH/s/XHASH/\#/' \
                       -e '/\@\@$$/s/\@\@$$/\\/'

# config data

rstart_DATA = config

config: config.cpp
	$(AM_V_GEN) $(RAWCPP) $(RAWCPPFLAGS) -DPACKAGEname=rstart \
	  -DLIBDIR=$(rstartdir) -DENVPREFIX=RSTART \
	  < ${srcdir}/config.cpp | $(CPP_SED_MAGIC) > $@

# wrapper scripts

bin_SCRIPTS = rstart rstartd

rstart: client.cpp
	$(AM_V_GEN) $(RAWCPP) $(RAWCPPFLAGS) -DRSHCMD=$(RSH) \
	  < ${srcdir}/client.cpp | $(CPP_SED_MAGIC) > $@

rstartd: server.cpp
	$(AM_V_GEN) $(RAWCPP) $(RAWCPPFLAGS) -DBINDIR=$(rstartdir) \
	  -DLIBDIR=$(rstartdir) < ${srcdir}/server.cpp | \
	  $(CPP_SED_MAGIC) > $@

EXTRA_DIST = client.cpp server.cpp config.cpp

MAINTAINERCLEANFILES = ChangeLog INSTALL
CLEANFILES = rstart rstartd config

#
# commands and contexts
#

# FIXME: Someone who cares about rstart may want to look into sanitizing
# the stuff below.

install-data-hook:
	(cd ${srcdir};							\
	for name in `find ./commands -print | grep -v CVS` ; do		\
	    if test -f $$name; then					\
	        sed							\
		    -e 's,ENVPREFIX,RSTART,g'				\
		        < $$name > $(DESTDIR)$(rstartdir)/$$name;	\
		case `basename $(DESTDIR)$(rstartdir)/$$name` in		\
		@*)							\
		    ;;							\
		*)							\
		    chmod a+x $(DESTDIR)$(rstartdir)/$$name ;;		\
		esac;							\
	    else							\
	        if [ -d $$name ] ; then					\
	            mkdir -p $(DESTDIR)$(rstartdir)/$$name ;		\
	        fi;							\
	    fi;								\
	done;								\
									\
	for name in `find ./contexts -print | grep -v CVS` ; do		\
	    if test -f $$name; then					\
	        sed							\
	            -e 's,_PATH,$(DEFAULT_USER_PATH),g'			\
	            -e 's,_MANPATH,$(DEFAULT_X_MANPATH),g'		\
			< $$name > $(DESTDIR)$(rstartdir)/$$name ;	\
	    else							\
	        if [ -d $$name ] ; then					\
	            mkdir -p $(DESTDIR)$(rstartdir)/$$name	;	\
	        fi ;							\
	    fi ;							\
	done;								\
	);								\
									\
	cd $(DESTDIR)$(rstartdir)/contexts;				\
	sed -e '/^$$/d' -e '/^#/d' @Aliases |				\
	    while read real aliases; do					\
	        for i in $$aliases; do					\
	            $(RM) $$i;						\
	            $(LN) $$real $$i;					\
	            $(RM) $(DESTDIR)$(rstartdir)/commands/$$i;		\
	            $(LN) $$real $(DESTDIR)$(rstartdir)/commands/$$i; 	\
	        done;							\
	    done;							\
	$(RM) @Aliases

distuninstallcheck:
	@:

EXTRA_DIST +=								\
	server.os2							\
									\
	commands/ListContexts						\
	commands/x11r6/Terminal						\
	commands/x11r6/LoadMonitor					\
	commands/x11r6/@List						\
	commands/@List							\
	commands/ListGenericCommands					\
									\
	contexts/default						\
	contexts/@Aliases						\
	contexts/x11r6							\
	contexts/@List							\
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
