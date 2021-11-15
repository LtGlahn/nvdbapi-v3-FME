NVDB api V2 with FME
===============
Sample FME workspaces to fetch objects from [NVDB api v3](https://nvdbapiles-v3.atlas.vegvesen.no/dokumentasjon/). 

Made with FME desktop 2020.1.2.1, all later versions should work. 

Modifying these workspace to fetch other object types is straightforward, but the property names must be explisit defined (or exposed) in an attributeCreator transformer (or attribute Exposer, if you prefer). With over 360 object types defined in NVDB data catalogue, this handiwork could become very boring and tedious very quickly. 

Please note that the location of the xfsmap definition file (*xfmapDefintion_nvdbapi2fme_V2.xml*) in the XMLFeatureReader transformer must be adjusted to match your local file system. In most cases, it should work straight out of the box - if not, just pop open the "point at file" dialog, click on the file and hit OK.

![Locate xfmapfile in XML Feature M](/images/locate_xfmapfile.PNG)
 
# nvdbapi_v3_bruksklasse.fmw

Sample workspace, fetching traffic TO BE WRITTEN data for a geographical area (bounding)

# nvdbapi_v3_trafikkmengde



# SLAKT det under 


# nvdbapi_V2_Bomstasjoner.fmw 

Will download all toll stations (Bomstasjoner) from NVDB api. Optionally, you may also augment the data set with street names. If enablet, for each Bomstasjon-feature we attempt to fetch street names at the bomstasjon locations in the road network(NVDB object "Gate", property "Gatenavn" at the same route ID + road network position). See https://www.vegvesen.no/nvdb/apidokumentasjon/#/verdi/veglenke 

> **Note** Due to a bug in NVDB api, we cant search for objects at a **precise** point at the road network: We must search within a stretch of the road. So instead of the _linkID@position_ syntax we  use _linkID@fromPosition-toPosition_ 
 

Make sure the location of the xfsmap defintion file (*xfmapDefintion_nvdbapi2fme_V2.xml*) in the XMLFeatureReader transformer matches your local file system. 

# nvdbapi_V2_bruksklasse.fmw

Downloads "871 bruksklasse", and will also segment each object into smaller pieces where appropriate (no multilinestring-geometry). Each segment  has uniqe vegreferanse (road reference) hp-meter and roadlink - roadlinkposition values. 


# nvdbapi_V2_trafikkulykker.fmw 

Will download road accidents (trafikkulykke) from NVDB api for Tromsø kommune. Optionally, only download accidents with property  _Alvorligste skadegrad = drept_

Make sure the location of the xfsmap defintion file (*xfmapDefintion_nvdbapi2fme_V2.xml*) in the XMLFeatureReader transformer matches your local file system. 


# nvdbapi_V2_bru_vegbru.fmw 

Will download all "bru" features satisfying these filters: 
* Brukategori (bridge category) = Vegbru 
* Length >= 20 meter or not defined

Bruer (bridges) can be quite an handfull to process correctly. Many bridges are attatched (stedfestet) to many road links at once. We show one way of dealing with this kind of problems: Pry apart the individual segments, and (hopefully) it is easier to use. (LineString instead of MultiLineString geometry, one road number and not a list of road numbers etc). The drawback is that each NVDB object may be be represented several times, one time for each individual road. 

![Example of NVDB bru attatched to multiple roads](/images/bru-multippelstedfesting.png)

NVDB object [547334236](https://www.vegvesen.no/nvdb/api/v2/vegobjekter/60/547334236.xml) is attatched (stedfestet) both to Ev18 and Fv305. 

# nvdbapi_V2_turistveger_2geojson.fmw 

Reads object type 777 Turistveger (tourist roads) from NVDB api and writes to 3 geojson-files: 
 * Full geometry, 7mb
 * Generalized with about 100m tolerance (0.001 degrees), 96kb
 * Generalized with about 1000m tolerance (0.01 degrees), 23kb
 
The generalisation method is the FME default [Ramer-Douglas-Peuker algorithm] (https://en.wikipedia.org/wiki/Ramer%E2%80%93Douglas%E2%80%93Peucker_algorithm) (also known as Dogulas or Douglas-Peuker). 

