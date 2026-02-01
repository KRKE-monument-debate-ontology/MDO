---
description: (what are the questions that we are able to answer with our ontology?
icon: question
---

# Competency Questions

#### <sub>_possiamo dire che quelle che mostriamo come risposte sono una semplificazione_</sub>&#x20;

### PREFIXES&#x20;

{% code expandable="true" %}
```sparql
PREFIX  pr: <http://www.ontologydesignpatterns.org/cp/owl/participantRole.owl>
PREFIX  crm: <http://www.cidoc-crm.org/cidoc-crm/> 
PREFIX  dbo: <http://dbpedia.org/ontology/> 
PREFIX  deo: <http://purl.org/spar/deo/> 
PREFIX  dio: <https://w3id.org/dio#> 
PREFIX  mdo: <https://github.com/KRKE-monument-debate-ontology/Data_MDO/md-ontology/> 
PREFIX  ns1: <http://w3id.org/CEON/ontology/material/> 
PREFIX  owl: <http://www.w3.org/2002/07/owl#> 
PREFIX  rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
PREFIX  tip: <http://ontologydesignpatterns.owl/cp/owl/timeindexedparticipation.owl/> 
PREFIX  xml: <http://www.w3.org/XML/1998/namespace> 
PREFIX  xsd: <http://www.w3.org/2001/XMLSchema#> 
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#> 
PREFIX  skos: <http://www.w3.org/2004/02/skos/core#> 
PREFIX  time: <http://www.w3.org/2006/time#> 
PREFIX  dcterms: <http://purl.org/dc/terms/> 
PREFIX  schema: <http://schema.org/> 
PREFIX  ceon-actor: <http://w3id.org/CEON/ontology/actor/> 
```
{% endcode %}

### 1) QUERY&#x20;

{% code expandable="true" %}
```sparql
SELECT 
  ?monument
  (SAMPLE(?title) AS ?titleSample)
  (SAMPLE(?date) AS ?dateSample)
  (SAMPLE(?HistoricalFigureLabel) AS ?HistoricalFigureLabelSample)
  (GROUP_CONCAT(DISTINCT ?materialLabel; separator=", ") AS ?materials)
  (GROUP_CONCAT(DISTINCT ?locationLabel; separator=", ") AS ?locations)
  (GROUP_CONCAT(DISTINCT ?creatorLabel; separator=", ") AS ?creators)
  (GROUP_CONCAT(DISTINCT ?funderLabel; separator=", ") AS ?funders)
  (GROUP_CONCAT(DISTINCT ?featureLabel; separator=", ") AS ?features)
  (GROUP_CONCAT(DISTINCT ?contextualLabel; separator=", ") AS ?contextualizations)
  (GROUP_CONCAT(DISTINCT ?conceptLabel; separator=", ") AS ?concepts)
  (GROUP_CONCAT(DISTINCT ?controversyLabel; separator=", ") AS ?controversies)

WHERE {
  ?monument rdf:type dbo:Monument .
  OPTIONAL { ?monument dcterms:title ?title . }
  OPTIONAL { ?monument dcterms:date ?date . }
  OPTIONAL { ?monument ns1:hasMaterialComponent ?material .
    OPTIONAL { ?material rdfs:label ?materialLabel . }}
  OPTIONAL { ?monument schema:location ?location .
    OPTIONAL { ?location rdfs:label ?locationLabel . }}
  OPTIONAL { ?monument schema:creator ?creator .
    OPTIONAL { ?creator rdfs:label ?creatorLabel . }}
  OPTIONAL { ?monument schema:funder ?funder .
    OPTIONAL { ?funder rdfs:label ?funderLabel . }}
  OPTIONAL { ?monument crm:P56 ?feature .
    OPTIONAL { ?feature rdfs:label ?featureLabel . }}
  OPTIONAL { ?monument mdo:isContextualizedBy ?contextual .
    OPTIONAL { ?contextual rdfs:label ?contextualLabel . }}
  OPTIONAL { ?monument mdo:reflectsHeritageOf ?concept .
    OPTIONAL { ?concept rdfs:label ?conceptLabel . }}
  OPTIONAL { ?monument mdo:triggeredControversy ?controversy .
    OPTIONAL { ?controversy rdfs:label ?controversyLabel . }}
  OPTIONAL { ?monument crm:P62 ?HistoricalFigure .
    OPTIONAL { ?HistoricalFigure rdfs:label ?HistoricalFigureLabel . }}
}
GROUP BY ?monument
```
{% endcode %}

<table data-full-width="true"><thead><tr><th>title</th><th width="153.5999755859375">HistoricalFigure</th><th>date</th><th>material</th><th>location</th><th width="100">creator</th><th>funder</th><th>feature</th><th width="171.20001220703125">contextual material</th><th width="176">heritage concept</th><th width="146.4000244140625">controversy</th></tr></thead><tbody><tr><td>Indro Montanelli Statue</td><td>Indro Montanelli (1909-2001)</td><td>2006</td><td>Bronze</td><td>Montanelli public gardens, Milan</td><td>Vito Tongiani</td><td>Municipality of Milan</td><td>Indro Montanelli seated while typing on his Olivetti</td><td>Engraved inscription on the pedestal reading "Indro Montanelli, Journalist"</td><td>Colonialism, Freedom of press, Pedophilia, Racism</td><td>The controversy triggered by Indro Montanelli Statue in the montanelli public gardens</td></tr></tbody></table>

***

### 2) QUERY

{% code fullWidth="false" %}
```sparql
SELECT ?historicalFigureLabel ?legacyLabel ?controversialFactLabel
WHERE {
  ?monument crm:P62 ?historicalFigure .
  OPTIONAL { 
    ?historicalFigure rdfs:label ?historicalFigureLabel . }
  OPTIONAL {
    ?historicalFigure mdo:hasLegacyImpact ?legacy .
    OPTIONAL { ?legacy rdfs:label ?legacyLabel . }}
  OPTIONAL {
    ?historicalFigure schema1:performerIn ?controversialFact .
    OPTIONAL { ?controversialFact rdfs:label ?controversialFactLabel . }}
}
```
{% endcode %}

<table><thead><tr><th width="251.20001220703125">historicalFigureLabel</th><th width="247.79998779296875">legacyLabel</th><th width="338.5999755859375">controversialFactLabel</th></tr></thead><tbody><tr><td>Mahatma Gandhi (1869-1948)</td><td>Opposition to racism, Indian Independence, Non violent resistance</td><td>Racist and derogatory statements about Black Africans in early writings, Casteist views and ambiguous position on the indian's caste system</td></tr><tr><td>Carl Hagenbeck (1844-1913)</td><td>Animal welfare, Zoo design</td><td>Hagenbeck was known for his exhibitions of people, especially from Africa, which were brought in Germany and displayed in circuses and zoos</td></tr></tbody></table>
