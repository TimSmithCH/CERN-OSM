# CERN-OSM
Mapping CERN building numbers to Open Street Map object IDs enables buildings to be located on an OSM simply using their number.
This repository contains the code to generate these mappings, together with the latest output in a JSON file.

## Principle
- CERN buildings appear on [Open Street Maps](http://www.openstreetmap.org) since their descriptions have been imported from the Cadastre Francais
  on the French territoy and the SIG on the Swiss
- Buildings are stored as OSM "ways" which trace their outer perimeters
  - Example: [B28](http://www.openstreetmap.org/way/23696046)
- The outer perimeter of each CERN site is also stored as an OSM "way"
  - Example: [Meyrin Site](http://www.openstreetmap.org/way/174126176)
- The OSM metadata is stored in a DB which can be queried using the [Overpass API](http://overpass-api.de/api/interpreter)
- Using the overpass API it is possible to identify all ways within the CERN perimeter

## Prctice
- Run the Overpass API to identify CERN buildings:
- http://overpass-api.de/api/interpreter?data=[out:json];way[%22building%22=%22yes%22](46.2292395,%206.0338419,46.2385931,6.0570547);out%20meta;
- Output to a file called "buildings_in_CERN_bb.json"
- Run build_osm_mappings.py on it to strip out just the building number-osm id pairs
- Output to a file called "cern_osm_mappings.json"

## Notes
- Can also chain such a call to visualise maps:
- http://overpass-api.de/api/interpreter?data=[out:custom];way[%22building%22=%22yes%22][%22name%22=%22281%22](46.2292395,%206.0338419,46.2385931,6.0570547);out;&url=http://www.openstreetmap.org/?{{{type}}}={{{id}}}
