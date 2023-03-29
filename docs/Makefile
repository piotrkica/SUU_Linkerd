pdfdocs: docs.tex
	@make clean
	@pdflatex docs.tex
	@pdflatex docs.tex
	@bibtex docs.aux
	@pdflatex docs.tex
	@pdflatex docs.tex

.PHONY: ed view clean

ed:
	@vim docs.tex

view:
	@evince docs.pdf

clean:
	@rm -f *.pdf *.toc *.aux *.log *.bbl *.blg
