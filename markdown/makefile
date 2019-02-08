.PHONY: help all
.DEFAULT_GOAL := help

LOCATION=https://gist.githubusercontent.com/rvente/89c0bb1c8e1f454411c331500d6c0587

# makefile modified 2018 12 16 🄯copyleft Ralph Vente
# support multi file input, Darwin and Linux Platforms
# usage: by default, inputs all md files in current directory
# if a `md=some_File` is supplied, only that file will be generated

ifndef md
DOCFILE=$(wildcard *.md)
DOCNAME=CSCI-260-Notes
else
DOCFILE=$(md)
DOCNAME=$(basename $(DOCFILE))
endif

HEADERFILE=$(wildcard *.yaml)
NOTESFILE=$(wildcard *note*.md)
UNAME_S=$(shell uname -s)
MLA=mlaTemplateDocument.tex

STYLING = "PDF styling is"

DATE=$(shell date +"%Y-%m-%d")

# for mac/linux compatability
# assumes template doc is present in pandoc templates directory on mac
# assumes template doc is present in ~/Templates/MLA directory on linux

ifeq ($(UNAME_S),Darwin)
				MLA_TEMPLATE=$(MLA)
		else
				MLA_TEMPLATE=$(HOME)/Templates/MLA/$(MLA)
		endif

# create new md file with yyyy-mm-dd or append date to existing file as heading1
new:
	echo "# $(shell echo $(DATE) | sed "s/-/ /g")" >> $(DATE).md


# export each file into its own individual pdf
each:
	set -e; for FILE in $(DOCFILE);\
          do for NAME in $(basename $(DOCFILE));\
            do pandoc $(HEADERFILE) -s $$FILE -o\
							$$NAME.pdf ;\
            done \
          done

# output pdf of all converted *.md unless `make md=file.md` is specified
default:
	pandoc $(HEADERFILE) -s $(DOCFILE)\
		-o $(DOCNAME).pdf
	@echo "Export complete. $(DOCNAME).pdf present in current dir."
	@echo "$(STYLING) default."

# default with mla format
mla:
	pandoc $(HEADERFILE) -s $(DOCFILE)\
		-o $(DOCNAME).pdf\
		--template=$(MLA_TEMPLATE)
	@echo "Export complete. $(DOCNAME).pdf present in current dir."
	@echo "$(STYLING) mla."

# exports files matching *note*.md only
notes:
	pandoc $(HEADERFILE) -s $(NOTESFILE)\
		-o $(basename $(NOTESFILE)).pdf
	@echo "Export complete. $(basename $(NOTESFILE)).pdf present in current dir."
	@echo "$(STYLING) default."

# `make 2018-09-28.pdf` should produce 2018-09-28.pdf with default style
%.pdf: %.md
	pandoc $(HEADERFILE) -s $<\
		-o $(basename $<).pdf
	@echo "Export complete. $(basename $<).pdf present in current dir."
	@echo "$(STYLING) default."

# `make 2018-09-28.mla` should produce 2018-09-28.pdf with mla style
%.mla: %.md
	DOCNAME=$(basename $<)
	pandoc $(HEADERFILE) -s $<\
		-o $(basename $<).pdf
		--template=$(MLA_TEMPLATE)
	@echo "Export complete. $(basename $<).pdf present in current dir."
	@echo "$(STYLING) mla."

# sync copy of this makefile from github
update:
	curl -o makefile -O $(LOCATION)/raw

# allonym for notes
note: notes

# Show this help.
help:
	@awk '/^#/{c=substr($$0,3);next}c&&/^[[:alpha:]][[:alnum:]_-]+:|\%/{print substr($$1,1,index($$1,":")),c}1{c=0}' $(MAKEFILE_LIST) | column -s: -t

# attrib:
#   help function: with modifications
#      https://stackoverflow.com/questions/35730218/how-to-automatically-generate-a-makefile-help-command
