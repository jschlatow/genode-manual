SHELL := bash

default:
	@echo Create PDF from individual Tikz image
	@echo -e "\nusage: make <name.pdf>\n"


invoke_pdflatex = lualatex -jobname $* \
                    "\input{tikz-standalone.tex} \input{$<} \end{document}"

%.pdf: %.tikz
	$(invoke_pdflatex)
	$(invoke_pdflatex)
	$(invoke_pdflatex)

flye%.pdf: flye%.tex
	lualatex $<
	lualatex $<
	lualatex $<

flye%.pdf: app_specific_tcb.pdf

qrcode_%.pdf: qrcode_%.eps
	epspdf $< $@

qrcode_linkedin.eps:
	qrencode -o $@ -t eps -s 6 -m 0 -l M https://de.linkedin.com/company/genodelabs

qrcode_mastadon.eps:
	qrencode -o $@ -t eps -s 6 -m 0 -l M https://floss.social/@genode

flyer.pdf: qrcode_linkedin.pdf

flyer_community.pdf: qrcode_mastadon.pdf

flyer_a5.pdf: flyer.pdf
	cpdf -scale-to-fit "154mm 216mm" $< -o $@

flyer_community_a5.pdf: flyer_community.pdf
	cpdf -scale-to-fit "154mm 216mm" $< -o $@

#
# Make each target depend on common included files and the Makefile
#
TIKZ_FILES := $(wildcard *.tikz)
PDF_FILES  := $(TIKZ_FILES:.tikz=.pdf)

$(PDF_FILES): tikz-common.tex tikz-preamble.tex tikz-standalone.tex Makefile
