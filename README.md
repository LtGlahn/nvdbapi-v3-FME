Hent data fra NVDB api V3 med FME
===============
Noen FME workspace-eksempler for å hente data fra [NVDB api v3 LEs](https://nvdbapiles-v3.atlas.vegvesen.no/dokumentasjon/). 

Laget med FME desktop 2020.1.2.1, alt nyere enn det skulle funke fint. 

Når du modifiserer disse workspacene for å bli bedre tilpasset dine behov så må du endre både objekttype (`published parameter = objektTypeID`) og eksplisitt definere hvilke egenskapsverdier som du skal ha med ved å editere den fremhevede `attributeCreator`. Her må du manuelt legge til egenskapsnavn for de egenskapene som er mulige for denne objekttypen. Egenskapsnavnene (og hva de betyr) finner du i [NVDB datakatalog](https://datakatalogen.vegdata.no/)

> Å editere egenskapsnavn manuelt på denne måten funker helt fint for ad-hoc oppgaver, men du trøtner fort om du skal laste ned et stort antall objekttyper. 
> For å ikke å snakke om vedlikeholdsbehovet når det skjer datakatalogendringer. For eksempel et navnebytte er nok til at du ikke lenger får lastet inn den 
> egenskapsverdien i workspace. 
> 
> Ideelt sett hadde vi hatt en datakatalogdrevet skjemadefinisjon, enten fra [GML schemas](https://github.com/vegvesen/NVDB-Datakatalogen/tree/master/GML) 
> eller ved å bruke [/vegobjekter - endepunktet til NVDB api LES](https://nvdbapiles-v3.atlas.vegvesen.no/dokumentasjon/openapi/#/Datakatalog/get_vegobjekttyper). 
> Gode idéer og andre bidrag mottas med takk, gjerne i form av pull requests med nye workspace-eksempler.  

# Tips og tricks 

I workspace har vi tilrettelagt to dataflyter, med sine styrker og svakheter. 
  * En flyt som behandler **hele NVDB-objektet samlet**. Denne egner seg best for fysisk vegutstyr som rekkverk, men det finnes også en del andre brukstilfeller der det er gunstig med en feature per NVDB objekt
  * En flyt som **deler opp objektene i individuelle vegsegmenter**. Dette vil som regel være en naturlig arbeidsflyt for abstrakte data som fartsgrenser og bruksklasse, men kan også passe hvis man for eksempel skal se på overlapp langs vegnettet for fysisk vegutstyr _(for eksempel rekkverk og fartsgrenser - rekkverk har jo som regel egengeometri til siden for vegkanten, mens fartsgrenser er limt oppå senterlinja. Ved å bruke vegsegmenter-arbeidsflyten kan man finne felles overlapp langs senterlinja)_. Hvert vegsegment har detaljer om stedfesting og vegsystemreferanse, og ett NVDB objekt er da komponert av ett eller flere vegsegmenter. 

Det kan hende at du må riste litt før workspace ditt klarer finne `xfsmap`-definisjonsfila (`xfmapDefintion_nvdbapi2fme_V3.xml`) i  `XMLFeatureReader`-transformeren 
(den som er markert med _"this is where the magic happens"_  ). 
Ideelt sett finner FME xfsmap-fila av seg selv fordi den ligger i samme mappen, men det kan hende du må peke på den manuelt på ditt filsystem. 
Kjør først workspace, og hvis det tryner så åpner du   "pek-på-xfsmap-fil" dialogen.

![Locate xfmapfile in XML Feature M](/images/locate_xfmapfile.PNG)
 

# nvdbapi_v3_bruksklasse.fmw

Eksempel workspace som henter  [904 Bruksklasse normaltransport](https://datakatalogen.vegdata.no/904-Bruksklasse,%20normaltransport) for Skaun kommune, Trøndelag. 

# nvdbapi_v3_trafikkmengde - historiske data

Henter NVDB data for  [540 trafikkmengde](https://datakatalogen.vegdata.no/540-Trafikkmengde) for Arendal kommune, Agder, for datoen  (`tidspunkt=2019-04-01`), i tillegg til dagens dataverdier. 

