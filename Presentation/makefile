# TODO: remove old presentation.html rule, adapt onlinepres to publish rule, add new scope event in which the Presentation folder is only one element, add .gitignore
EVENTNAME = $(shell pwd | grep -oP '(\w|-)+' | tail -n 2 | head -n 1 | tr '[:upper:]' '[:lower:]')
.PHONY: all init publish test

all: publish 

init: 
	@echo remove .svn repository folder
	-rm -rf .svn
	@echo clone font-awesome fonts
	@curl https://raw.githubusercontent.com/FortAwesome/Font-Awesome/master/css/font-awesome.min.css > Includes/FontAwesome/font-awesome.min.css
	@echo clone reveal.js github repository
	@git clone git@github.com:hakimel/reveal.js.git
	@echo remove .git repository folder
	@rm -rf reveal.js/.git

publish:
	@echo Copy Slides to Presentation Folder on Server
	mkdir $(EVENTNAME)
	cp index.html $(EVENTNAME)/index.html	
	cp -r ./reveal.js $(EVENTNAME)/
	if [ -d "./Includes" ]; then cp -r ./Includes $(EVENTNAME)/; fi
	if [ -d "./Overrides" ]; then cp -r ./Overrides $(EVENTNAME)/; fi
	rsync --progress --delete -rae ssh $(EVENTNAME) cutuchiqueno@web385.webfaction.com:webapps/htdocs/slides/
	
test:
	@echo ${PROJNAME}
	#
# clean: onlinepres
# 	rm -rf $(PROJNAME)

