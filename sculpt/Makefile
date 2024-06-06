TARGET = sculpt.pdf
SHELL  = bash

CHAPTERS_TXT := sculpt.txt

default: $(TARGET)

show: $(TARGET)
	xdg-open $(TARGET)

$(CHAPTERS_TXT:.txt=.tex): misc.gosh

%.tex: %.txt
	gosh --style misc.gosh $< > $@

%.pdf: %.tex
	pdflatex $<
	pdflatex $<
	pdflatex $<

print.pdf: $(TARGET:.pdf=.txt) print.gosh Makefile $(wildcard *.png) | img
	gosh --style print.gosh $(TARGET:.pdf=.txt) > print.tex
	pdflatex print.tex
	pdflatex print.tex
	pdflatex print.tex

cover.pdf: cover.txt cover.gosh Makefile img img/cover_sculpt.tikz
	gosh --style cover.gosh cover.txt > cover.tex
	pdflatex cover.tex

img:
	ln -sf ../img

clean cleanall:
	rm -f *.tex *~ *.aux *.log *.toc *.out *.brf *.blg *.bbl $(TARGET)
	rm -f print.pdf

colorpages: print.pdf
	gs -q -o - -sDEVICE=inkcov print.pdf | grep -v "^ 0.00000  0.00000  0.00000" | wc -l
