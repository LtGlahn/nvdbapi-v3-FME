NVDB api V3 with FME
===============
Sample FME workspaces to fetch objects from [NVDB api v3](https://nvdbapiles-v3.atlas.vegvesen.no/dokumentasjon/). 

Made with FME desktop 2020.1.2.1, all later versions should work. 

Modifying these workspace to fetch other object types is straightforward, but the property names must be explisit defined (or exposed) in an attributeCreator transformer (or attribute Exposer, if you prefer). With over 360 object types defined in [NVDB data catalogue](https://datakatalogen.vegdata.no/), this handiwork works very well for ad-hoc needs, but will become somewhat tedious if you want to download a large number of different feature types - not to ignore that you'd need to manually scrutinize for changes to the NVDB data catalogue and make appropriate manual adjustments. Generic, schema-driven solutions (derived from either [GML schemas](https://github.com/vegvesen/NVDB-Datakatalogen/tree/master/GML) or from [/vegobjekter - endpoint of NVDB api LES](https://nvdbapiles-v3.atlas.vegvesen.no/dokumentasjon/openapi/#/Datakatalog/get_vegobjekttyper) ) would be highly preferable. Pull requests with such solutions are welcome. 

Please note that the location of the xfsmap definition file (`xfmapDefintion_nvdbapi2fme_V3.xml`) in the XMLFeatureReader transformer must be adjusted to match your local file system. In most cases, it should work straight out of the box - if not, just pop open the "point at file" dialog, click on the file and hit OK.

![Locate xfmapfile in XML Feature M](/images/locate_xfmapfile.PNG)
 
# nvdbapi_v3_bruksklasse.fmw

Sample workspace, fetching NVDB data for  [904 Bruksklasse normaltransport](https://datakatalogen.vegdata.no/904-Bruksklasse,%20normaltransport) the municipality of Skaun, Trøndelag. 

# nvdbapi_v3_trafikkmengde - historical data

Fetching NVDB data for  [540 trafikkmengde](https://datakatalogen.vegdata.no/540-Trafikkmengde) for the municipality of Arendal, Agder, for the date (`tidspunkt=2019-04-01`), as well as today's values. 

