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

# rstartd server

rstart_serverdir = $(libdir)/X11/rstart
rstart_server_PROGRAMS = rstartd.real

rstartd_real_CFLAGS = $(RSTART_CFLAGS)			\
		-DSERVERNAME=\"rstart\"			\
		-DDEFAULT_CONFIG=\"$(libdir)/X11/rstart/config\"

rstartd_real_LDADD = $(RSTART_LIBS)

rstartd_real_SOURCES =	\
        auth.c \
        server.c

# config data

configdir = $(libdir)/X11/rstart
config_DATA = config

config: config.cpp
	$(CPP) -DPACKAGEname=rstart -DLIBDIR=$(configdir) -DENVPREFIX=RSTART ${srcdir}/config.cpp |	\
		$(SED) -e /^\#.*$$/d  -e s/XCOMM/\#/g   > $@

# wrapper scripts

bin_SCRIPTS = rstart rstartd

rstart: client.cpp
	$(CPP) -DRSHCMD=$(RSH) -DSERVERNAME=rstartd ${srcdir}/client.cpp |	\
		$(SED) -e /^\#.*$$/d  -e s/XCOMM/\#/g   > $@

rstartd: server.cpp
	$(CPP) -DBINDIR=$(rstart_serverdir) -DLIBDIR=$(configdir) \
		${srcdir}/server.cpp |	\
		$(SED) -e /^\#.*$$/d  -e s/XCOMM/\#/g   > $@

# man pages

dist_man1_MANS = \
        rstartd.man \
        rstart.man

EXTRA_DIST = client.cpp server.cpp config.cpp

CLEANFILES = rstart rstartd config

#
# commands and contexts
#

# FIXME: Someone who cares about rstart may want to look into sanitizing
# the stuff below.

DATA_DIR = $(libdir)/X11/rstart

install-data-hook:
	for name in `find ${srcdir}/commands -print | grep -v CVS` ; do	\
	    if test -f $$name; then					\
	        sed							\
		    -e 's,ENVPREFIX,RSTART,g'				\
		        < $$name > $(DATA_DIR)/$$name;			\
		case `basename $(DATA_DIR)/$$name` in			\
		@*)							\
		    ;;							\
		*)							\
		    chmod a+x $(DATA_DIR)/$$name ;;			\
		esac;							\
	    else							\
	        if [ -d $$name ] ; then					\
	            mkdir -p $(DATA_DIR)/$$name ;			\
	        fi;							\
	    fi;								\
	done;								\
									\
	for name in `find ${srcdir}/contexts -print | grep -v CVS` ; do	\
	    if test -f $$name; then					\
	        sed							\
	            -e 's,_PATH,$(DEFAULT_USER_PATH),g'			\
	            -e 's,_MANPATH,$(DEFAULT_X_MANPATH),g'		\
				< $$name > $(DATA_DIR)/$$name ;		\
	    else							\
	        if [ -d $$name ] ; then					\
	            mkdir -p $(DATA_DIR)/$$name	;			\
	        fi ;							\
	    fi ;							\
	done;								\
									\
	cd $(DATA_DIR)/contexts;					\
	sed -e '/^$$/d' -e '/^#/d' ${srcdir}/contexts/@Aliases |	\
	    while read real aliases; do					\
	        for i in $$aliases; do					\
	            $(RM) $$i;						\
	            $(LN) $$real $$i;					\
	            $(RM) $(DESTDIR)$(rstart_serverdir)/commands/$$i;	\
	            $(LN) $$real $(DESTDIR)$(rstart_serverdir)/commands/$$i; \
	        done;							\
	    done;							\
	$(RM) @Aliases

distuninstallcheck:
	@:

EXTRA_DIST +=								\
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
	samples/contexts.odt1/openwindows3
