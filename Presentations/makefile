PROJNAME = $(shell pwd | grep -oP '(\w|-)+' | tail -n 2 | head -n 1 | tr '[:upper:]' '[:lower:]')

# all: $(PROJNAME).html _presentation.html onlinepres clean

presentation.html: presentation.md css/niels.css
	@echo Compile Test Presentation from Markdown File
	@pandoc --data-dir=$(HOME)/.pandoc --template=slides/revealjs_niels_tmpl.html --css=./css/niels.css -V revealjs-url:./resources/revealjs -V theme:serif -V transition:slide presentation.md -f markdown -t revealjs -o presentation.html


# $(PROJNAME).html: presentation.md
#	@echo Compile Presentation from Markdown File
#	@pandoc --self-contained --data-dir=$(HOME)/.md --template=slides/revealjs_niels_tmpl.html --css=./css/niels.css -V revealjs-url:$(HOME)/.md/revealjs -V theme:serif presentation.md -f markdown -t revealjs -o $(PROJNAME).html
# presentation.html: presentation.md css/niels.css
#	@echo Compile Presentation from Markdown File
#	@pandoc --self-contained --data-dir=$(HOME)/.md --template=slides/revealjs_niels_tmpl.html --css=./css/niels.css -V revealjs-url:./resources/revealjs -V theme:serif -V transition:slide presentation.md -f markdown -t revealjs -o presentation.html

onlinepres: presentation.html
	@echo Copy Slides to Presentation Folder on Server
	mkdir $(PROJNAME)
	if [ -d "./resources" ]; then cp -r ./resources $(PROJNAME)/; fi
	if [ -d "./css" ]; then cp -r ./css $(PROJNAME)/; fi
	cp presentation.html $(PROJNAME)/index.html	
	rsync --progress --delete -rae ssh $(PROJNAME) cutuchiqueno@web385.webfaction.com:webapps/htdocs/slides/

clean: onlinepres
	rm -rf $(PROJNAME)

bootstrap: 
	git clone git@github.com:hakimel/reveal.js.git
