# Incomplete list of queries and source for files

> Trivia: (label).(UN m49).(source).(extension) where
> - UN m49 = https://en.wikipedia.org/wiki/UN_M49
> - source = wikidata (Wikidata.org), osm (OpenStreetMap.org), ...


## hospital.076.osm.geojson

```c
[out:json][timeout:60];
//{{geocodeArea:Rio Grande do Sul}}->.searchArea;
{{geocodeArea:Brazil}}->.searchArea;
nwr["amenity"="hospital"](area.searchArea);
out center meta;
```

## hospital.001.wikidata.tsv
```sparql
#Map of hospitals
#defaultView:Map
SELECT DISTINCT ?item ?itemLabel ?countryLabel ?official_website ?geo WHERE {
  ?item (wdt:P31/(wdt:P279*)) wd:Q16917;
    wdt:P625 ?geo.
  OPTIONAL { ?item wdt:P856 ?official_website. }
  OPTIONAL { ?item wdt:P17 ?country. }
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],pt,en,de,fr,ru,es,ja,zh,ar". }
}
ORDER BY (xsd:integer(STRAFTER(STR(?item), "entity/Q")))
```

## countries.001.osm.geojson

```c
[out:json][timeout:25];
relation["boundary"="administrative"]["admin_level"="2"];
out center tags;
```

##

```
# command geojsonedit:
#   see https://github.com/fititnt/spatial-data-conflation-open-toolchain
#   pip install --upgrade gis-conflation-toolchain
cat d/countries.001.osm.geojson | geojsonedit --format-output=geojsonseq -
cat d/countries.001.osm.geojson | geojsonedit --output-type=GeoJSONSeq - > d/countries.001.osm.geojsonl
```

## countries-territories-unocha.001.hxl.csv
- https://data.humdata.org/dataset/countries-and-territories?
- https://docs.google.com/spreadsheets/d/1NjSI2LaS3SqbgYc0HdD8oIb7lofGtiHgoKKATCpwVdY/edit?gid=1088874596#gid=1088874596

## countries.001wikidata.tsv

```sparql
# SELECT ?country (STRAFTER(STR(?country), "entity/") as ?wikidata) ?unm49 ?iso3166n ?iso3166p1a2 ?iso3166p1a3 ?osmrelid ?unescot ?usciafb ?usfips4 ?gadm

SELECT ?country (STRAFTER(STR(?country), "entity/") as ?wikidata) ?P2082 ?P299 ?P297 ?P298 ?osmrelid ?unescot ?usciafb ?usfips4 ?gadm ?geo

WHERE
{
  ?country wdt:P31 wd:Q6256 ;
  OPTIONAL { ?country wdt:P2082 ?P2082. }    # ?unm49
  OPTIONAL { ?country wdt:P299 ?P299. }    # ?iso3166n
  OPTIONAL { ?country wdt:P297 ?P297. } # ?iso3166p1a2
  OPTIONAL { ?country wdt:P298 ?P298. } # ?iso3166p1a3
  OPTIONAL { ?country wdt:P402 ?osmrelid. }    # ?osmrelid
  OPTIONAL { ?country wdt:P3916 ?unescot. }    # ?unescot
  OPTIONAL { ?country wdt:P9948 ?usciafb. }    # ?usciafb
  OPTIONAL { ?country wdt:P901 ?usfips4. }     # ?usfips4
  OPTIONAL { ?country wdt:P8714 ?gadm. }       # gadm
  
  # @TODO get P498 (ISO 4217 code) instead of Q for currency
  # OPTIONAL { ?country wdt:P38 ?P38 . } # ?iso3166p1a3
  
  OPTIONAL { ?country wdt:P625 ?geo. }  
  
  #not a former country
  FILTER NOT EXISTS {?country wdt:P31 wd:Q3024240}
  #and no an ancient civilisation (needed to exclude ancient Egypt)
  FILTER NOT EXISTS {?country wdt:P31 wd:Q28171280}

  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]". }
}

ORDER BY (xsd:integer(STRAFTER(STR(?item), "entity/Q")))
```