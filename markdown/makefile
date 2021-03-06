.PHONY: help all
.DEFAULT_GOAL := help

# 🄯 copyleft Ralph Vente
# support multi file input, Darwin and Linux Platforms

ifndef md
DOCFILE=$(wildcard *.md)
DOCNAME=CSCI-260-Notes-Full
else
DOCFILE=$(md)
DOCNAME=$(basename $(DOCFILE))
endif

HEADERFILE=$(wildcard *.yaml)

STYLING = "PDF styling is"

DATE=$(shell date +"%Y-%m-%d")

# create new md file with yyyy-mm-dd or append date to existing file as heading1
new:
	echo "# $(shell echo $(DATE) | sed "s/-/ /g")" >> $(DATE).md

# output single pdf of all converted *.md into ../pdf/
all:
	pandoc $(HEADERFILE) -s *.md\
            -o ../pdf/$(DOCNAME).pdf
	@echo "Export complete. $(DOCNAME).pdf present in pdf directory."
	@echo "$(STYLING) default."

# `make 2018-09-28.pdf` should produce 2018-09-28.pdf with default style
%.pdf: %.md
	pandoc $(HEADERFILE) -s $<\
            -o ../pdf/$(basename $<).pdf
	@echo "Export complete. $(basename $<).pdf present in pdf directory."
	@echo "$(STYLING) default."

# export each file into its own individual pdf
each:
	@for FILE in $(basename $(DOCFILE)); do \
            pandoc $(HEADERFILE) -s $$FILE.md -o ../pdf/$$FILE.pdf ;\
        done
	@echo "Export complete. PDF's with various filenames present in ../pdf directory."
	@echo "$(STYLING) default."


# Show this help.
help:
	@awk '/^#/{c=substr($$0,3);next}c&&/^[[:alpha:]][[:alnum:]_-]+:|\%/{print substr($$1,1,index($$1,":")),c}1{c=0}' $(MAKEFILE_LIST) | column -s: -t

# attrib:
#   help function: with modifications
#      https://stackoverflow.com/questions/35730218/how-to-automatically-generate-a-makefile-help-command
