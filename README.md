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
    "timestamp_osm_base": "20XX-XX-XXTXX:XX:XXZ",
    "timestamp_areas_base": "20XX-XX-XXTXX:XX:XXZ",
    "copyright": "The data included in this document is from www.openstreetmap.org. The data is made available under ODbL."
    },
    "elements": [{}...{}]}
  - New Structure: [{ "version": 0.6, "generator": "Overpass API 0.7.62.7 375dc00a", "timestamp_osm_base": "20XX-XX-XXTXX:XX:XXZ", "timestamp_areas_base": "20XX-XX-XXTXX:XX:XXZ", "copyright": "The data included in this document is from www.openstreetmap.org. The data is made available under ODbL."}, {}...{}]
- Removed "type": "node", "way" and "relation" entries from each object
- Removed "center": entries from each way or relation object
- Promoted key-value pairs from tags object into the parent object
- Promoted key-value pairs from center object into the parent object
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

No other data was added or combined.

## Queries to Overpass Turbo

- All Motorways junctions in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["highway"="motorway_junction"](area.de);way["highway"="motorway_junction"](area.de);relation["highway"="motorway_junction"](area.de););out center;

- All Campings in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["tourism"="camp_site"](area.de);way["tourism"="camp_site"](area.de);relation["tourism"="camp_site"](area.de););out center;

- All DHL Stations in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["name"~"dhl|postfiliale",i](area.de);node["brand"~"dhl",i](area.de);node["operator"~"dhl",i](area.de);way["name"~"dhl|postfiliale",i](area.de);way["brand"~"dhl",i](area.de);way["operator"~"dhl",i](area.de);relation["name"~"dhl|postfiliale",i](area.de);relation["brand"~"dhl",i](area.de);relation["operator"~"dhl",i](area.de););out center;

- All Hermes Stations in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["name"~"hermes",i]["name"!~"restaurant",i]["name"!~"hermeskeil",i]["name"!~"hermesplatz",i]["name"!~"hermesstraße",i](area.de);node["brand"~"hermes",i](area.de);node["operator"~"hermes",i](area.de);way["name"~"hermes",i]["name"!~"restaurant",i](area.de);way["brand"~"hermes",i](area.de);way["operator"~"hermes",i](area.de);relation["brand"~"hermes",i](area.de);relation["operator"~"hermes",i](area.de););out center;

- All Amazon Stations in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["name"~"amazon",i](area.de);node["brand"~"amazon",i](area.de);node["operator"~"amazon",i](area.de);way["name"~"amazon",i](area.de);way["brand"~"amazon",i](area.de);way["operator"~"amazon",i](area.de);relation["name"~"amazon",i](area.de);relation["brand"~"amazon",i](area.de);relation["operator"~"amazon",i](area.de););out center;

- All Car rental services in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["amenity"="car_rental"](area.de);way["amenity"="car_rental"](area.de);relation["amenity"="car_rental"](area.de););out center;

- All train stations in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["railway"="station"](area.de);way["railway"="station"](area.de);relation["railway"="station"](area.de););out center;

- All bus stations in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["highway"="bus_stop"](area.de);way["highway"="bus_stop"](area.de);relation["highway"="bus_stop"](area.de););out center;

- All tram stations in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["railway"="tram_stop"](area.de);way["railway"="tram_stop"](area.de);relation["railway"="tram_stop"](area.de););out center;

- All Pharmacies in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"]->.de;(node["amenity"="pharmacy"](area.de);way["amenity"="pharmacy"](area.de);relation["amenity"="pharmacy"](area.de););out center;

- All Bike Shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="bicycle"](area.de);way["shop"="bicycle"](area.de);relation["shop"="bicycle"](area.de););out center;

- All Beverage Shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="beverages"](area.de);way["shop"="beverages"](area.de);relation["shop"="beverages"](area.de););out center;

- All swimming pools in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["leisure"="water_park"](area.de);way["leisure"="water_park"](area.de);relation["leisure"="water_park"](area.de););out center;

- All bakeries in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="bakery"](area.de);way["shop"="bakery"](area.de);relation["shop"="bakery"](area.de););out center;

- All travel agents in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="travel_agency"](area.de);way["shop"="travel_agency"](area.de);relation["shop"="travel_agency"](area.de););out center;

- All tyre shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="tyres"](area.de);way["shop"="tyres"](area.de);relation["shop"="tyres"](area.de););out center;

- All night clubs in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["amenity"="nightclub"](area.de);way["amenity"="nightclub"](area.de);relation["amenity"="nightclub"](area.de););out center;

- All bars in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["amenity"="bar"](area.de);way["amenity"="bar"](area.de);relation["amenity"="bar"](area.de););out center;

- All pubs in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["amenity"="pub"](area.de);way["amenity"="pub"](area.de);relation["amenity"="pub"](area.de););out center;

- All universities in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["amenity"="university"](area.de);way["amenity"="university"](area.de);relation["amenity"="university"](area.de););out center;

- All schools in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["amenity"="school"](area.de);way["amenity"="school"](area.de);relation["amenity"="school"](area.de););out center;

- All kindergartens in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["amenity"="kindergarten"](area.de);way["amenity"="kindergarten"](area.de);relation["amenity"="kindergarten"](area.de););out center;

- All book stores in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="books"](area.de);way["shop"="books"](area.de);relation["shop"="books"](area.de););out center;

- All golf clubs in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["leisure"="golf_course"](area.de);way["leisure"="golf_course"](area.de);relation["leisure"="golf_course"](area.de););out center;

- All kiosks in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="kiosk"](area.de);way["shop"="kiosk"](area.de);relation["shop"="kiosk"](area.de););out center;

- All pet shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="pet"](area.de);way["shop"="pet"](area.de);relation["shop"="pet"](area.de););out center;

- All beauty shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"~"chemist|perfumery"](area.de);way["shop"~"chemist|perfumery"](area.de);relation["shop"~"chemist|perfumery"](area.de););out center;

- All Supermarkets in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="supermarket"](area.de);way["shop"="supermarket"](area.de);relation["shop"="supermarket"](area.de););out center;

- All Car Dealers in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="car"](area.de);way["shop"="car"](area.de);relation["shop"="car"](area.de););out center;

- All Car Repair shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="car_repair"](area.de);way["shop"="car_repair"](area.de);relation["shop"="car_repair"](area.de););out center;

- All Motorbike Dealers in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="motorcycle"](area.de);way["shop"="motorcycle"](area.de);relation["shop"="motorcycle"](area.de););out center;

- All fitness centers in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["leisure"="fitness_centre"](area.de);way["leisure"="fitness_centre"](area.de);relation["leisure"="fitness_centre"](area.de););out center;

- All furniture shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="furniture"](area.de);way["shop"="furniture"](area.de);relation["shop"="furniture"](area.de););out center;

- All DIY shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="doityourself"](area.de);way["shop"="doityourself"](area.de);relation["shop"="doityourself"](area.de););out center;

- All Electronics and computer shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["shop"="computer"](area.de);node["shop"="electronics"](area.de);way["shop"="computer"](area.de);way["shop"="electronics"](area.de);relation["shop"="computer"](area.de);relation["shop"="electronics"](area.de););out center;

- All Lotto shops in Germany:

  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["name"~"lotto",i](area.de);node["brand"~"lotto",i](area.de);node["operator"~"lotto",i](area.de);node["shop"="lottery"](area.de);node["lotto"="yes"](area.de);node["lottery"="yes"](area.de);node["description"~"lotto",i](area.de);node["shop"~"kiosk|newsagent"]["lotto"="yes"](area.de);node["shop"~"kiosk|newsagent"]["lottery"="yes"](area.de);way["name"~"lotto",i](area.de);way["brand"~"lotto",i](area.de);way["operator"~"lotto",i](area.de);way["shop"="lottery"](area.de);way["lotto"="yes"](area.de);way["lottery"="yes"](area.de);way["description"~"lotto",i](area.de);way["shop"~"kiosk|newsagent"]["lotto"="yes"](area.de);way["shop"~"kiosk|newsagent"]["lottery"="yes"](area.de);relation["name"~"lotto",i](area.de);relation["brand"~"lotto",i](area.de);relation["operator"~"lotto",i](area.de);relation["shop"="lottery"](area.de);relation["lotto"="yes"](area.de);relation["lottery"="yes"](area.de);relation["description"~"lotto",i](area.de);relation["shop"~"kiosk|newsagent"]["lotto"="yes"](area.de);relation["shop"~"kiosk|newsagent"]["lottery"="yes"](area.de););out center;

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
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["amenity"="fast_food"]["name"~"kfc|mcdonald|^burger king|subway|^pizza hut|nordsee|backwerk|vapiano|starbucks",i](area.de);way["amenity"="fast_food"]["name"~"kfc|mcdonald|^burger king|subway|^pizza hut|nordsee|backwerk|vapiano|starbucks",i](area.de);relation["amenity"="fast_food"]["name"~"kfc|mcdonald|^burger king|subway|^pizza hut|nordsee|backwerk|vapiano|starbucks",i](area.de););out center;

- All street & fast food in Germany, chains excluded (Imbiss):
  - https://overpass-api.de/api/interpreter?data=[out:json];area["ISO3166-1"="DE"][admin_level=2]->.de;(node["amenity"="fast_food"]["name"!~"kfc|mcdonald|^burger king|subway|^pizza hut|nordsee|backwerk|vapiano|starbucks",i](area.de);way["amenity"="fast_food"]["name"!~"kfc|mcdonald|^burger king|subway|^pizza hut|nordsee|backwerk|vapiano|starbucks",i](area.de);relation["amenity"="fast_food"]["name"!~"kfc|mcdonald|^burger king|subway|^pizza hut|nordsee|backwerk|vapiano|starbucks",i](area.de););out center;

## License

These datasets are released under the [Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/1-0/).
Data are derived from www.openstreetmap.org

You are free to:

- Copy, modify and use the data for any purpose
- Share adapted versions under the same license (ODbL)
- Use the data in Produced Works (e.g., visualizations, reports) — just include attribution to OpenStreetMap

See full license text: https://opendatacommons.org/licenses/odbl/1-0/
