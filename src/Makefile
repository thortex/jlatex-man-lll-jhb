.SUFFIXES: .tex .dvi .pdf .obj .eps .aux .bb .tgz .tar .gz .out \
	.bz2 .tbz .zip .jpg .jpeg .bmp .png .log .toc .lof .lot \
	.idx .glo .ilg .glg .gls .ltx .dtx .bib .bbl .blg

VERSION	=	1.0.0

# programs
# for Mac OS X 
EDITOR	=	openemacs
XDVI	=	open -a Mxdvi.app
XPDF	=	open -a Preview.app
# using utf8 (euc (E), jis (J), sjis (S) )
TEX	=	platex -kanji=utf8 -src-specials 
BIBTEX	=	jbibtex -kanji=utf8
MAKEIDX	=	mendex -g -U -d hb.dic
EBB	=	ebb
DVIPDF	=	dvipdfmx
REFGREP	=	grep "^LaTeX Warning: Label(s) may have changed."

# directories
SRC_DIR	=	src
IMG_DIR	=	img
STY_DIR	=	mcr

# source files
MAIN	=	hb
SRC	=	Makefile jhb.sty
IST	=	$(STY_DIR)/sind.ist
GST	=	$(STY_DIR)/sgls.ist
BST	=	$(STY_DIR)/splain.bst
# for distributable archive file
CODENAME=	jhb
DISTDIR	=	jhb

# aliases
all:	fast
dvi:	$(MAIN).dvi
pdf:	$(MAIN).pdf
bib:	$(MAIN).bbl
ind:	$(MAIN).ind $(MAIN).gls
fast:	$(MAIN).tex
	$(TEX) $(MAIN) && $(TEX) $(MAIN)

$(MAIN).dvi:	$(SRC) $(MAIN).aux $(MAIN).gls $(MAIN).ind #$(MAIN).bbl 
$(MAIN).pdf:	$(MAIN).dvi
$(MAIN).aux:	$(MAIN).tex
$(MAIN).bbl:	$(MAIN).bib 
$(MAIN).bib:	$(MAIN).aux
$(MAIN).ind:	$(MAIN).idx
$(MAIN).gls:	$(MAIN).glo
$(MAIN).idx:	$(MAIN).aux
$(MAIN).glo:	$(MAIN).aux

$(MAIN).ind:	$(MAIN).idx
	$(MAKEIDX) -s $(IST) -t $*.ilg -o $@ $<
$(MAIN).gls:	$(MAIN).glo
	$(MAKEIDX) -s $(GST) -t $*.glg -o $@ $<

# translation rules
.dvi.pdf:
	$(DVIPDF) $<
.tex.dvi:
	$(TEX) $<
	$(TEX) $<
	(while grep "^No file $*.lo" $*.log; do $(TEX) $<; done)
	(while $(REFGREP) $*.log; do $(TEX) $<; done)
.bib.bbl:
	$(BIBTEX) $*
.tex.aux:
	$(TEX) $<
	(while grep "^No file $*.lo" $*.log; do $(TEX) $<; done)
	(while $(REFGREP) $*.log; do $(TEX) $<; done)

# preview
view:	$(MAIN).dvi
	$(XDVI) $< & 
viewpdf:	$(MAIN).pdf
	$(XPDF) $< & 
# edit
edit:	
	$(EDITOR) $(MAIN).tex & 

clean: 
#	touch hb.{aux,log,toc,out,lof,lot,idx,ilg,ind,glo,glg,gls,bbl,blg,tmp,exa}
	rm -f hb.{aux,log,toc,out,lof,lot,idx,ilg,ind,glo,glg,gls,bbl,blg,tmp,exa}

cleanall:	clean	
	rm -f hb.{dvi,pdf}
	rm -f *~
	rm -f $(SRC_DIR)/*~
	rm -f $(SRC_DIR)/*.aux
	rm -f $(IMG_DIR)/*~
	rm -f $(STY_DIR)/*~

distpdf:	pdf
	mv $(MAIN).pdf ../$(CODENAME)-$(VERSION).pdf

dist:	cleanall 
	mkdir -p $(DISTDIR)/{$(SRC_DIR),$(IMG_DIR),$(STY_DIR)}
	cp -r $(SRC_DIR) $(IMG_DIR) $(STY_DIR) $(DISTDIR)
	cp $(MAIN).tex $(MAIN).dic $(MAIN).bib  $(SRC) $(DISTDIR)
	tar cjf ../$(DISTDIR)-$(VERSION).tar.bz2 $(DISTDIR)
	rm -fr $(DISTDIR)

help:
	@echo -e "\nusage: make [<target>]\n"
	@echo -e "all:\tDVI ファイル $(MAIN).dvi を作成する"
	@echo -e "bib:\t文献ファイル $(MAIN).bbl を作成する"
	@echo -e "clean:\t一時ファイルを全て削除する"
	@echo -e "dist:\tバックアップ $(DISTDIR).tar.bz2 を一つの上のフォルダに作成する"
	@echo -e "dvi:\tDVI ファイル $(MAIN).dvi を作成する"
	@echo -e "edit:\tテキストエディッタ $(EDITOR) で $(MAIN).tex を開く\t"
	@echo -e "fast:\t依存関係を無視して $(MAIN).tex をコンパイルする"
	@echo -e "help:\tこのメッセージを表示する"
	@echo -e "pdf:\tPDF ファイル $(MAIN).pdf を作成する"
	@echo -e "view:\tDVI を $(XDVI) で開く"
	@echo -e "viewpdf: PDF を $(XPDF) で開く"
	@echo -e "viewcfg: 設定情報を閲覧する\n"

viewcfg:
	@echo -e "VERSION:\t$(VERSION)"
	@echo -e "CODENAME:\t$(CODENAME)"
	@echo -e "EDITOR:\t$(EDITOR)"
	@echo -e "XDVI:\t$(XDVI)"
	@echo -e "XPDF:\t$(XPDF)"
	@echo -e "TEX:\t$(TEX)"
	@echo -e "BIBTEX:\t$(BIBTEX)"
	@echo -e "MAKEIDX:\t$(MAKEIDX)"
	@echo -e "EBB:\t$(EBB)"
	@echo -e "DVIPDF:\t$(DVIPDF)"
	@echo -e "SRC_DIR:\t$(SRC_DIR)"
	@echo -e "IMG_DIR:\t$(IMG_DIR)"
	@echo -e "STY_DIR:\t$(STY_DIR)"
	@echo -e "MAIN:\t$(MAIN)"
	@echo -e "SRC:\t$(SRC)"

bb:
	(cd $(IMG_DIR) && $(EBB) *.pdf)

