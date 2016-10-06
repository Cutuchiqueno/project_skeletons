EVENTNAME = $(shell pwd | grep -oP '(\w|-)+' | tail -n 2 | head -n 1 | tr '[:upper:]' '[:lower:]')

.PHONY: all init publish archive

all: publish slides.pdf archive

init: 
	@echo remove .svn repository folder
	-rm -rf .svn
	@echo clone font-awesome fonts
	@curl https://raw.githubusercontent.com/FortAwesome/Font-Awesome/master/css/font-awesome.min.css > Includes/FontAwesome/font-awesome.min.css
	@echo clone reveal.js github repository
	@git clone git@github.com:hakimel/reveal.js.git
	@echo remove .git repository folder
	@rm -rf reveal.js/.git
	@mkdir reveal.js/plugin/printRevealNotes
	wget https://gist.githubusercontent.com/geek1011/1841acca111b7dbb3cd902497f255ba0/raw/fc3f7d5541859cf1a4abf7eafec63b06ac081d98/PrintRevealNotes.js

publish:
	@echo Copy Slides to Presentation Folder on Server
	mkdir $(EVENTNAME)
	cp index.html $(EVENTNAME)/index.html	
	cp -r ./reveal.js $(EVENTNAME)/
	if [ -d "./Includes" ]; then cp -r ./Includes $(EVENTNAME)/; fi
	if [ -d "./Overrides" ]; then cp -r ./Overrides $(EVENTNAME)/; fi
	rsync --progress --delete -rae ssh $(EVENTNAME) cutuchiqueno@web385.webfaction.com:webapps/htdocs/slides/
	rm $(EVENTNAME)

slides.pdf: index.html
	curl -L https://github.com/astefanutti/decktape/archive/v1.0.0.tar.gz | tar -xz --exclude phantomjs
	curl -L https://github.com/astefanutti/decktape/releases/download/v1.0.0/phantomjs-linux-x86-64 -o phantomjs
	chmod +x phantomjs
	./phantomjs ./decktape-1.0.0/decktape.js reveal index.html slides.pdf
	rm -rf decktape-1.0.0 phantomjs

archive: index.html
	# commands are merged into one line by backslash because otherwise the
	# user input is lost (one shell per command)
	-@read -p "Presentation title: " TITLE; \
	read -p "Conference: " CONFERENCE; \
	read -p "Date: " DATE; \
	read -p "Place: " PLACE; \
	read -p "Hashtag: " HASH; \
	echo "- [$$TITLE](http://nowalkowski.de/slides/$(EVENTNAME)/) ([Source](presentations/$(EVENTNAME)/index.html)) - _$$CONFERENCE, $$DATE, $$PLACE_" >> /home/cutuchiqueno/Research/GitHub-Output/README.md; \
	mkdir /home/cutuchiqueno/Research/GitHub-Output/presentations/$(EVENTNAME); \
	cp -r Includes/ Overwrites/ index.html reveal.js/ slides.pdf /home/cutuchiqueno/Research/GitHub-Output/presentations/$(EVENTNAME)/; \
	# t update "my presentation: $$TITLE on https://github.com/Cutuchiqueno/output/ #jtdh2016" FUNKTIONIERT NOCH NICHT
	-cd /home/cutuchiqueno/Research/GitHub-Output/ && git add . && git commit -a -m "add presentation for $(EVENTNAME)" && git push
