SUBDIRS = src include tools

if ENABLE_TESTS
SUBDIRS+=tests
endif

RPMDIR = RPMBUILD

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = libtrap.pc

ACLOCAL_AMFLAGS = -I m4
EXTRA_DIST = README README.ifcspec.md mdconvert.sh doc/index.txt doc/history.txt doc/index-devel.txt doc/develop-interface.txt

include aminclude.am

if DX_COND_doc
.PHONY: doxygen-devel
doxygen-devel:
	rm -rf devel-docs/
	doxygen devel-doxyfile

doc: doxygen-doc doxygen-devel

install-data-local:
	mkdir -p "$(DESTDIR)$(docdir)" && cp -R doc/doxygen/* "$(DESTDIR)$(docdir)" || echo "Documentation was not generated yet."
endif

if MAKE_RPMS

RPMFILENAME=$(PACKAGE_NAME)-$(VERSION)
.PHONY: rpm
rpm:
	rm -rf "$(RPMDIR)/SOURCES/$(RPMFILENAME)"
	mkdir -p $(RPMDIR)/BUILD/ $(RPMDIR)/SRPMS/ $(RPMDIR)/RPMS/ $(RPMDIR)/SOURCES
	make ${AM_MAKEFLAGS} distdir='$(RPMDIR)/SOURCES/$(RPMFILENAME)' distdir
	( cd "$(RPMDIR)/SOURCES/"; tar -z -c -f $(RPMFILENAME)-$(RELEASE).tar.gz $(RPMFILENAME); rm -rf $(RPMFILENAME); )
	$(RPMBUILD) -ba $(PACKAGE_NAME).spec --define "_topdir `pwd`/$(RPMDIR)";

rpm-clean:
	rm -rf $(RPMDIR)
else
endif

changelog:
	echo -e "This will insert history into ChangeLog file and open NEWS in vim.\nC-c to interrupt."; \
	git log --date=short --format="%cd (%an): %s" $(shell git tag | head -1)..HEAD . > chlog.tmp; \
	echo "" >> chlog.tmp; \
	cat ChangeLog >> chlog.tmp; \
	vim -o NEWS chlog.tmp; \
	mv chlog.tmp ChangeLog

