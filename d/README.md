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

