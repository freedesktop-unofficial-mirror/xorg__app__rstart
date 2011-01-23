
contextsdir = $(rstartdir)/contexts
contexts_DATA =		\
	x11r6

dist_contexts_DATA = 	\
	default		\
	@List

dist_noinst_DATA =	\
	@Aliases	\
	x11r6.in

PATH_SUBSTS = \
	-e 's|@DEFAULT_X_MANPATH[@]|$(DEFAULT_X_MANPATH)|g' \
	-e 's|@DEFAULT_USER_PATH[@]|$(DEFAULT_USER_PATH)|g'

x11r6: x11r6.in
	$(AM_V_GEN)$(SED) $(PATH_SUBSTS) < $< > $@

CLEANFILES = $(contexts_DATA)
