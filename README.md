# CERN-OSM
Mapping CERN building numbers to Open Street Map object IDs enables buildings to be located on an OSM simply using their number.
This repository contains the code to generate these mappings, together with the latest output in a JSON file.

## Principle
- CERN buildings appear on [Open Street Maps](http://www.openstreetmap.org) since their descriptions have been imported
  from the Cadastre Francais on the French territory and the SwissTopo on the Swiss territory
- Buildings are stored as OSM "ways" which trace their outer perimeters
  - Example: [B28](http://www.openstreetmap.org/way/23696046)
- The outer perimeter of each CERN site is also stored as an OSM "way"
  - Examples: [Meyrin Site](http://www.openstreetmap.org/way/174126176),
    [Prevessin site](http://www.openstreetmap.org/way/23722021),
    [P1 ATLAS](http://www.openstreetmap.org/way/340213340),
    [P2 ALICE](http://www.openstreetmap.org/way/253791392),
    [P5 CMS](http://www.openstreetmap.org/way/26966729),
    [P8 LHCb](http://www.openstreetmap.org/way/26099050)
- The OSM metadata is stored in a DB which can be queried using the [Overpass API](http://overpass-api.de/api/interpreter)
- Using the overpass API it is possible to identify all buildings within the CERN perimeter

## Practice
- Run the Overpass API to identify CERN buildings:
  - Use an area query (for Meyrin site) and select only buildings:
    - http://overpass-api.de/api/interpreter?data=[out:json][timeout:25];area(3600027005)-%3E.searchArea;(way[%22building%22=%22yes%22](area.searchArea););out%20meta;
    - N.b: an overpass area id is the osm id plus 2400000000 (for a way) or plus 3600000000 (for a relation)
- Queries for all CERN sites are contained in the bash script: osm_building_queries
- Run build_osm_mappings.py on the output files to strip out just the building number-osm id pairs
- Output to a file called "cern_osm_mappings.json"

## Notes
- Can also chain such a call to visualise maps:
  - http://overpass-api.de/api/interpreter?data=[out:custom];way[%22building%22=%22yes%22][%22name%22=%22281%22](46.2292395,%206.0338419,46.2385931,6.0570547);out;&url=http://www.openstreetmap.org/?{{{type}}}={{{id}}}
