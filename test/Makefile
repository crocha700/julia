JULIAHOME = $(abspath ..)
include ../Make.inc

TESTS = all linalg sparse $(filter-out TestHelpers runtests testdefs,$(subst .jl,,$(wildcard *.jl)))

default: all

$(TESTS) ::
	@$(call PRINT_JULIA, $(call spawn,$(JULIA_EXECUTABLE)) --check-bounds=yes --startup-file=no ./runtests.jl $@)

perf:
	@$(MAKE) -C perf all

clean:
	@$(MAKE) -C perf $@
	-rm -f libccalltest.$(SHLIB_EXT) ccalltest

.PHONY: $(TESTS) perf clean

libccalltest.$(SHLIB_EXT): ccalltest.c
	$(CC) $(CFLAGS) $(DEBUGFLAGS) -O3 $< -fPIC -shared -o $@ $(LDFLAGS) -DCC=$(CC)

ccalltest: ccalltest.c
	$(CC) $(CFLAGS) $(DEBUGFLAGS) -O3 $< -o $@ $(LDFLAGS) -DCC=$(CC)
