# Cleaned OpenStreetMap-Collections

This repository contains curated datasets derived from OpenStreetMap data, focusing on Germany.
They are published in compliance with the Open Database License (ODbL).

## Source

Data was extracted via the Overpass API from OpenStreetMap.

## Modifications

Each dataset was:
- Queried using custom Overpass Turbo requests
- Exported as raw JSON
  
Cleaning involved:
- Removed wrapper metadata, resulting in a flat array
  - Old Structure: {
  "version": 0.6,
  "generator": "Overpass API 0.7.62.7 375dc00a",
  "osm3s": {
    "timestamp_osm_base": "2025-07-01T14:39:14Z",
    "timestamp_areas_base": "2025-07-01T10:52:18Z",
    "copyright": "The data included in this document is from www.openstreetmap.org. The data is made available under ODbL."
  },
  "elements": [{}...{}]}
  - New Structure: [{"generator": "Overpass API 0.7.62.7 375dc00a", "copyright": "The data included in this document is from www.openstreetmap.org. The data is made available under ODbL."}, {}...{}]
- Removed "type": "node" entries from each object
- Promoted key-value pairs from tags object into the parent object
- Standardized structure for easier processing

No other data was added or combined.

## Exceptions:
- Collection "postServices" that apart from the above cleaning process, was generated from three different queries listed below and:
  - Added to each object of the resulting array a key value:
    - "brand":"DHL", for DHL stations extraction
    - "brand":"Hermes", for Hermes stations extraction
    - "brand":"Amazon", for Amazon stations extraction
  - Removed irrelevant results 
  - Removed obvious duplicates
  - Combined the three cleaned extractions.
 
- Collection "electronicsShops" that apart from the above cleaning process, was generated from two different queries listed below and:
  - Combined the two extractions.

No other data was added or combined.

## Queries to Overpass Turbo

- All Motorway exits in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["highway"="motorway_junction"](area);out body;

- All Campings in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["tourism"="camp_site"]](area);out body;

- All post services in germany:
  - All DHL Stations in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["name"~"dhl|postfiliale",i](area.de);node["brand"~"dhl",i](area.de);node["operator"~"dhl",i](area.de););out center;

  - All Hermes Stations in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["name"~"hermes",i](area.de);node["brand"~"hermes",i](area.de);node["operator"~"hermes",i](area.de););out center;

  - All Amazon Stations in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["name"~"amazon",i](area.de);node["brand"~"amazon",i](area.de);node["operator"~"amazon",i](area.de););out center;

- All Car rental services in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="car_rental"](area);out body; 

- All train stations in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["railway"="station"](area);out body;

- All bus stations in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["highway"="bus_stop"](area);out body;

- All tram stations in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["railway"="tram_stop"](area);out body;

- All Pharmacies in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="pharmacy"](area);out body;

- All Bike Shops in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="bicycle"](area);out body;

- All Beverage Shops in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="beverage"](area);out body;

- All swimming pools in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["name"~"freibad|schwimmbad",i](area);out body;

- All bakeries in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="bakery"](area);out body;

- All travel agents in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="travel_agency"](area);out body;

- All tyre shops in Germany:
  - All Atu
  - All Pneuhage
  - All the rest
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["name"~"^reifen|^pneu|^atu",i](area);out body;
   
- All night clubs in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="nightclub"](area);out body;

- All bars in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="bar"](area);out body;

- All pubs in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="pub"](area);out body;

- All universities in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="university"](area);out body;

- All high schools in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="college"](area);out body;

- All schools in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="school"](area);out body;

- All kindergartens in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="kindergarten"](area);out body;

- All book stores in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="books"](area);out body; 

- All golf clubs in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["sport"="golf"](area);out body;

- All kiosks in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="kiosk"](area);out body;

- All pet shops in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="pet"](area);out body;

- All beauty shops in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"~"chemist|perfumery"](area);out body;

- All Supermarkets in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="supermarket"](area);out body; 

- All Car Dealers in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="car"](area);out body;

- All Car Repair shops in Germany:
  - All Bosch Car Service
  - All Carglass
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="car_repair"]["name"~"^carglass|^bosch",i](area);out body;

- All Motorbike Dealers in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="motorcycle"](area);out body;

- All fitness centers in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["leisure"="fitness_centre"](area);out body;

- All furniture shops in Germany:
  - All Höffner
  - All IKEA
  - All MöMax
  - All XXL Lutz
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["brand"~"ikea|xxxlutz|höffner|mömax",i](area);out body;

- All DIY shops in Germany:
  - All Bauhaus
  - All Globus Baumarkt
  - All Hagebau
  - All Hornbach
  - All OBI
  - All Toom Baumarkt
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["name"~"^obi|bauhaus|hornbach|baumarkt|toom",i](area);out body;

- All Electronics shops in Germany:
  - All MediaMarkt 
  - All Saturn
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["name"~"^saturn|^mediamarkt",i](area);out body;
  - All the others
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["shop"="computer"](area);out body;

- All Lotto shops in Germany:
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="lottery"](area.de);way["shop"="lottery"](area.de);node["name"~"lotto",i](area.de););out center;

- All fast food chains in Germany:
  - Backwerk
  - Burger King
  - KFC
  - McDonalds
  - Nordsee
  - Pizza Hut
  - Starbucks
  - Subway
  - Vapiano
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["name"~"kfc|mcdonald|^burger king|subway|^pizza hut|nordsee|backwerk|vapiano|starbucks",i](area);out body; 

- All Imbiss (street & fast food in Germany, chains excluded):
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2];node["amenity"="fast_food"]["name"!~"kfc|mcdonald|^burger king|subway|^pizza hut|nordsee|backwerk|vapiano|starbucks",i](area);out body;

## License

These datasets are released under the [Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/1-0/).
Data are derived from www.openstreetmap.org

You are free to:
- Copy, modify and use the data for any purpose
- Share adapted versions under the same license (ODbL)
- Use the data in Produced Works (e.g., visualizations, reports) — just include attribution to OpenStreetMap

See full license text: https://opendatacommons.org/licenses/odbl/1-0/
