TEX_FILES = $(wildcard tex/*.tex)
UNMOVED_TEX_FILES = $(wildcard *.tex)
PDF_FILES = $(TEX_FILES:tex/%.tex=pdf/%.pdf)
UNMOVED_PDF_FILES = $(UNMOVED_TEX_FILES:%.tex=pdf/%.pdf)
MOVE_MARKER = build/.moved

all: $(MOVE_MARKER) $(UNMOVED_PDF_FILES) $(PDF_FILES)

$(MOVE_MARKER):
	mkdir -p build tex pdf
	find ./ -type f -name '*.tex' -exec mv \{\} tex/ \;
	find . -maxdepth 1 -type f ! -name 'Makefile' ! -name '.gitignore' -exec rm {} \;
	find pdf -type f ! -name '*.pdf' -exec rm {} \;
	find tex -type f ! -name '*.tex' -exec rm {} \;
	touch $(MOVE_MARKER)

pdf/%.pdf: tex/%.tex
	mkdir -p pdf build
	pdflatex -output-directory=build $<
	mv build/$*.pdf pdf/

pdf/%.pdf: %.tex
	mkdir -p tex pdf
	mv $< tex/
	$(MAKE) pdf/$(notdir $(<:.tex=.pdf))

obsah: $(TEX_FILES)
	mkdir -p pdf build
	for file in $(TEX_FILES); do \
		pdflatex -output-directory=build $$file; \
		mv build/$$(basename -s .tex $$file).pdf pdf/; \
	done

clean:
	rm -rf build/*
	rm -f $(MOVE_MARKER)
	mkdir -p build tex pdf
	sh -c "find . -type f -name '*.tex' -exec mv \{\} tex/ \;"
	find . -maxdepth 1 -type f ! -name 'Makefile' ! -name '.gitignore' -exec rm {} \;
	find pdf -type f ! -name '*.pdf' -exec rm {} \;
	find tex -type f ! -name '*.tex' -exec rm {} \;

fclean: clean
	rm -rf pdf/*

re: fclean all

.PHONY: all clean fclean re