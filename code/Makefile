VERSION = 1.3
ZIPFILES = BETA/ Unicode/ README.txt
ZIPFILE = /tmp/Tischendorf-$(VERSION).zip
MQLZIPFILE = /tmp/Tischendorf8th_$(VERSION)_MQL.zip
ZIPDIR = Tischendorf-$(VERSION)
ZIPDIRPREFIX = /tmp
FULLZIPDIR = $(ZIPDIRPREFIX)/$(ZIPDIR)

CLEANFILES = out.txt *~ *.pyc *.TD2 *.TSP *.TUP \
             $(DERIVED_MQLFILES) $(DERIVED_TXTFILES) \
             README.txt 

DERIVED_MQLFILES = Byzantine.mql Stephens.mql \
                   tischendorfmorph.mql tischendorf.mql \
                   WH11.mql WH.mql whmorph.mql tisch.mql

DERIVED_TXTFILES = lexicon_nonunique.txt tischlexicon_nonunique.txt

#EMDROSPATH=/opt/EmdrosSQLite120pre140WX
#EMDROSPATH=/var/home/ulrikp/opt/EmdrosSQLite120pre143WX
EMDROSPATH=/var/home/ulrikp/opt/EmdrosSQLite120pre161-O3-noshared-WX-debug

DB = tisch

all:

clean: cleantisch
	rm -f $(CLEANFILES)

$(DB): tischendorfmorph.mql
	-echo "DROP DATABASE $(DB)" | $(EMDROSPATH)/bin/mql 
	echo "CREATE DATABASE $(DB)" | $(EMDROSPATH)/bin/mql 
	$(EMDROSPATH)/bin/mql -d $(DB) GNT_schema_1.2.mql
	$(EMDROSPATH)/bin/mql -d $(DB) -n tischendorfmorph.mql

wh: WH.mql
	-echo "DROP DATABASE wh" | $(EMDROSPATH)/bin/mql
	echo "CREATE DATABASE wh" | $(EMDROSPATH)/bin/mql 
	$(EMDROSPATH)/bin/mql -d wh GNT_schema_1.2.mql
	$(EMDROSPATH)/bin/mql -d wh -n WH.mql


cleantisch:
	-echo "DROP DATABASE $(DB)" | $(EMDROSPATH)/bin/mql 


tischclean: cleantisch

UnAccented: RE.TSP
	rm -rf UnAccented
	mkdir UnAccented
	for f in *.TSP; do NN=`echo $$f | sed -e 's/TSP/SLN/g'`; echo $$f; cat $$f | awk '{ print $$1 " " $$2 " " $$3;}' | sed -e 's/\([0-9]\+:[0-9]\+\).[0-9]\+/\1/' > UnAccented/$$NN; done

.PHONY: Accented

Accented: Accented/RE.SLN
	for f in Accented/*.SLN; do cat $$f | sed -e 's/\([0-9]\+\).[a-z]/\1/' > /tmp/SLN; mv /tmp/SLN $$f; done

zip: ../text/parsed/README.txt ./RE.TUP
	rm -f  $(ZIPFILE)
	rm -rf $(FULLZIPDIR)
	mkdir -p $(FULLZIPDIR)/BETA
	mkdir -p $(FULLZIPDIR)/Unicode
	cp ../text/parsed/README.txt $(FULLZIPDIR)
	for f in ../text/parsed/*.TSP; do NN=`echo $$f | sed -e 's/TSP/txt/g;s_../text/parsed/__g'`; cp $$f $(FULLZIPDIR)/BETA/$$NN; done
	for f in ./*.TUP; do NN=`echo $$f | sed -e 's/TUP/txt/g'`; cp $$f $(FULLZIPDIR)/Unicode/$$NN; done
	PWD=`pwd`; cd $(ZIPDIRPREFIX) ; zip -r $(ZIPFILE) $(ZIPDIR) ; cd $$PWD

README.txt : 
	cp $< $@


mqlzip: tisch.mql
	rm -f $(MQLZIPFILE)
	rm -rf $(FULLZIPDIR)
	mkdir -p $(FULLZIPDIR)
	cat README-MQL.txt ../text/parsed/README.txt > $(FULLZIPDIR)/README.txt
	cp tisch.cfg $(FULLZIPDIR)
	cp tisch.mql $(FULLZIPDIR)
	PWD=`pwd`; cd $(ZIPDIRPREFIX) ; zip -r $(MQLZIPFILE) $(ZIPDIR) ; cd $$PWD

tisch.mql: tischendorfmorph.mql
	rm -f tisch.mql
	echo "CREATE DATABASE tisch GO" > tisch.mql
	echo "USE DATABASE tisch GO" >> tisch.mql
	cat GNT_schema_1.2.mql >> tisch.mql
	cat tischendorfmorph.mql >> tisch.mql


