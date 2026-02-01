---
description: (what are the questions that we are able to answer with our ontology?
icon: question
---

# Competency Questions

### PREFIXES&#x20;

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
PREFIX  schema1: <http://schema.org/> 
PREFIX  ceon-actor: <http://w3id.org/CEON/ontology/actor/> 
```

### 2) QUERY

{% code fullWidth="false" expandable="true" %}
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

<table><thead><tr><th width="221.800048828125">historicalFigureLabel</th><th width="175">legacyLabel</th><th>controversialFactLabel</th></tr></thead><tbody><tr><td><strong>Mahatma Gandhi (1869-1948)</strong></td><td>Opposition to racism, Indian Independence, Non violent resistance</td><td>Racist and derogatory statements about Black Africans in early writings, Casteist views and ambiguous position on the indian's caste system</td></tr><tr><td><strong>Carl Hagenbeck (1844-1913)</strong></td><td>Animal welfare, Zoo design</td><td>Hagenbeck was known for his exhibitions of people, especially from Africa, which were brought in Germany and displayed in circuses and zoos</td></tr></tbody></table>
