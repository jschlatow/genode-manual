GENODE_VERSION = 24.05
PDF = genode-foundations-$(subst .,-,$(GENODE_VERSION)).pdf

all: $(PDF)

TEXFILE := misc.tmp
SHELL = bash

#
# Definition of the order of chapters
#
# Lines can be commented-out by prefixing them with '#'.
#
define CHAPTERS
title
introduction
part_1
getting_started
architecture
components
development
system_configuration
under_the_hood
part_2
#api
endef

default: $(PDF)

# concatenate chapters except for those that are commented out
$(PDF:.pdf=.txt): $(addsuffix .txt,$(patsubst #%,,$(strip $(CHAPTERS))))
	cat $^ > $@

$(PDF): $(PDF:.pdf=.txt) manual.gosh Makefile \
       $(wildcard img/*.pdf) \
       $(wildcard img/*.tikz) \
       spec/repos spec/classes
	gosh --style manual.gosh $(PDF:.pdf=.txt) > $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	cp $(TEXFILE).pdf $@
	rm -f $(TEXFILE) $(TEXFILE).*

$(PDF) print.pdf cover.pdf: img

print.pdf: $(PDF:.pdf=.txt) print.gosh Makefile part_2.png \
       $(wildcard img/*.pdf) \
       $(wildcard img/*.tikz)
	gosh --style print.gosh $(PDF:.pdf=.txt) > $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	cp $(TEXFILE).pdf $@
	rm -f $(TEXFILE) $(TEXFILE).*

cover.pdf: cover.txt cover.gosh Makefile img img/cover.tikz
	gosh --style cover.gosh cover.txt > $(TEXFILE).tex
	pdflatex $(TEXFILE).tex
	cp $(TEXFILE).pdf $@
	rm -f $(TEXFILE) $(TEXFILE).*

img:
	ln -sf ../img

clean_manual:
	rm -f $(TEXFILE) $(TEXFILE).* $(PDF:.pdf=.txt) $(PDF)
	rm -f img

spec/repos spec/classes:
	make -C spec all

clean_spec:
	make -C spec clean

clean: clean_manual clean_spec

colorpages: print.pdf
	gs -q -o - -sDEVICE=inkcov print.pdf | grep -v "^ 0.00000  0.00000  0.00000" | wc -l

part_2.png: Makefile
	qrencode --dpi=300 -l H -v 15 -o $@ https://genode.org/documentation/genode-foundations/24.05/api/index.html
