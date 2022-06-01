GENODE_VERSION = 23.05
PDF = genode-platforms-$(subst .,-,$(GENODE_VERSION)).pdf

all: $(PDF)

.PHONY: colorpages clean clean_manual

TEXFILE := misc.tmp
SHELL = bash

#
# Definition of the order of chapters
#
# Lines can be commented-out by prefixing them with '#'.
#
define CHAPTERS
title
intro
porting_guide
preparations
warmup
serial
kernel_skeleton
debugging
user_land
testing
device_access
platform_driver
linux
dts_pruning
networking
display
touchscreen
trimming_drivers
telephony
endef

default: $(PDF)

$(PDF) print.pdf: manual.gosh
	ln -s ../foundations

# concatenate chapters except for those that are commented out
$(PDF:.pdf=.txt): $(addsuffix .txt,$(patsubst #%,,$(strip $(CHAPTERS))))
	cat $^ > $@

$(PDF): $(PDF:.pdf=.txt) manual.gosh Makefile \
       $(wildcard img/*.pdf) \
       $(wildcard img/*.tikz)
	gosh --style manual.gosh $(PDF:.pdf=.txt) > $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	cp $(TEXFILE).pdf $@
	rm -f $(TEXFILE) $(TEXFILE).*

$(PDF) print.pdf cover.pdf: img

print.pdf: $(PDF:.pdf=.txt) print.gosh Makefile \
       $(wildcard img/*.pdf) \
       $(wildcard img/*.tikz)
	gosh --style print.gosh $(PDF:.pdf=.txt) > $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	lualatex $(TEXFILE).tex
	cp $(TEXFILE).pdf $@
	rm -f $(TEXFILE) $(TEXFILE).*

cover.pdf: cover.txt cover.gosh ../foundations/cover.gosh Makefile img img/cover.tikz
	gosh --style cover.gosh cover.txt > $(TEXFILE).tex
	pdflatex $(TEXFILE).tex
	cp $(TEXFILE).pdf $@
	rm -f $(TEXFILE) $(TEXFILE).*

img:
	ln -sf ../img

clean_manual:
	rm -f $(TEXFILE) $(TEXFILE).* $(PDF:.pdf=.txt) $(PDF)
	rm -f img
	rm -f cover.pdf
	rm -f print.pdf

clean: clean_manual

colorpages: print.pdf
	gs -q -o - -sDEVICE=inkcov print.pdf | grep -v "^ 0.00000  0.00000  0.00000" | wc -l
