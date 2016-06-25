.DEFAULT_GOAL := all

# TODO: Find idris executable.
IDRIS       ?= idris
IDRISFLAGS := --check --total
# TODO: Find pandoc executable.
PANDOC     ?= pandoc

LIDRFILES := Basics.lidr
# TODO: Add more chapters, in order, here.

IBCFILES  := $(LIDRFILES:.lidr=.ibc)
TEXFILES  := $(LIDRFILES:.lidr=.tex)

all: $(IBCFILES)


.PHONY: clean

clean:
	rm -rf all.{aux,log,out,toc,pdf} _minted-all $(IBCFILES) $(TEXFILES)

all.pdf: all.tex
	xelatex -shell-escape $<
	xelatex -shell-escape $<

all.tex: book.tex $(TEXFILES)
	$(PANDOC) --filter pandoc-minted.py --latex-engine=xelatex \
	-f markdown+lhs -t latex \
	-o $@ $(foreach tex,$(filter-out $<,$^),-A $(tex)) \
	--toc \
	book.tex

%.ibc: %.lidr
	$(IDRIS) $(IDRISFLAGS) $<

%.tex: %.lidr
	$(PANDOC) --filter pandoc-minted.py --latex-engine=xelatex \
	-f markdown+lhs -t latex+lhs \
	-o $@ $<