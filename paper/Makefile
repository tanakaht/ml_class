all: docx

HEADER=vsj19-header.md
MACRO=macros.sty
MARKDOWN=md/*-*.md
INPUTS=$(HEADER) $(MACRO) $(MARKDOWN)

DOCX=vsj19-tanaka.docx
HTML=vsj19-tanaka.html

define pandoc
  pandoc \
    --from markdown+latex_macros \
    --filter pandoc-citeproc \
    --bibliography=library.bib \
    $(INPUTS) \
    $(1)
endef
#    引用の形式を Chicogo 形式でなく APA 形式に変更する場合は以下の行を指定する。
#    --csl=etc/apa.csl \

docx: $(DOCX)
$(DOCX): $(INPUTS) etc/smartnova19.docx
	$(call pandoc, --reference-doc=etc/smartnova19.docx --output $@)

html: $(HTML)
$(HTML): $(INPUTS)
	$(call pandoc, --standalone --mathjax --css etc/page.css --output $@)

echohtml:
	@echo $(HTML)

clean:
	rm -f $(DOCX) $(HTML)
