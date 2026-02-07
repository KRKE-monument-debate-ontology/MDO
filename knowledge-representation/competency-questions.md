---
description: (what are the questions that we are able to answer with our ontology?
icon: question
---

# Competency Questions

#### <sub>_possiamo dire che quelle che mostriamo come risposte sono una semplificazione_</sub>&#x20;

### PREFIXES&#x20;

{% code fullWidth="false" expandable="true" %}
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

### LIVELLO 1 descrizione del monumento&#x20;

#### 1) Dove, quando, chi, come?

{% code fullWidth="false" expandable="true" %}
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
  ?monument a dbo:Monument .

  OPTIONAL { ?monument dcterms:title ?title . }
  OPTIONAL { ?monument dcterms:date ?date . }
  OPTIONAL {
    ?monument ns1:hasMaterialComponent ?material .
    ?material rdfs:label ?materialLabel .}
  OPTIONAL {
    ?monument schema:location ?location .
    ?location rdfs:label ?locationLabel .}
  OPTIONAL {
    ?monument schema:creator ?creator .
    ?creator rdfs:label ?creatorLabel .}
  OPTIONAL {
    ?monument schema:funder ?funder .
    ?funder rdfs:label ?funderLabel .}
  OPTIONAL {
    ?monument crm:P56 ?feature .
    ?feature rdfs:label ?featureLabel .}
  OPTIONAL {
    ?monument mdo:isContextualizedBy ?contextual .
    ?contextual rdfs:label ?contextualLabel .}
  OPTIONAL {
    ?monument mdo:reflectsHeritageOf ?concept .
    ?concept rdfs:label ?conceptLabel .}
  OPTIONAL {
    ?monument mdo:triggeredControversy ?controversy .
    ?controversy rdfs:label ?controversyLabel .}
  OPTIONAL {
    ?monument crm:P62 ?HistoricalFigure .
    ?HistoricalFigure rdfs:label ?HistoricalFigureLabel .}
}
GROUP BY ?monument
```
{% endcode %}

<table data-full-width="false"><thead><tr><th>title</th><th width="153.5999755859375">HistoricalFigure</th><th width="80.79998779296875">date</th><th width="104.79998779296875">material</th><th width="232.79998779296875">location</th><th width="129.5999755859375">creator</th><th>funder</th><th width="173.5999755859375">feature</th><th width="215.20001220703125">contextual material</th><th width="176">heritage concept</th><th width="222.4000244140625">controversy</th></tr></thead><tbody><tr><td>Indro Montanelli Statue</td><td>Indro Montanelli (1909-2001)</td><td>2006</td><td>Bronze</td><td>Montanelli public gardens, Milan</td><td>Vito Tongiani</td><td>Municipality of Milan</td><td>Indro Montanelli seated while typing on his Olivetti</td><td>Engraved inscription on the pedestal reading "Indro Montanelli, Journalist"</td><td>Colonialism, Freedom of press, Pedophilia, Racism</td><td>The controversy triggered by Indro Montanelli Statue in the montanelli public gardens</td></tr></tbody></table>

***

### LIVELLO 2 - valori simbolici legati alla statua e al personaggio storico

#### 2) QUERY Perché il personaggio è stato celebrato con una statua? (crm:E12  crm:P17  mdo:Legacy)

{% code fullWidth="false" %}
```sparql
SELECT ?historicalFigureLabel ?legacyLabel 
WHERE {
  ?monument crm:P62 ?historicalFigure .
  OPTIONAL { 
    ?historicalFigure rdfs:label ?historicalFigureLabel . }
  OPTIONAL {
    ?historicalFigure mdo:hasLegacyImpact ?legacy .
    ?legacy rdfs:label ?legacyLabel . }
  
```
{% endcode %}

#### 3) Quali valori e concetti sono associati alla statua? (mdo:Monument  mdo:reflectsHeritageOf  skos:Concept)

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

#### 4) Per quali motivi il personaggio raffigurato è considerato controverso ? (dbo:HistoricalFigure  schema:performerIn  mdo:ControversialFact)

***

### LIVELLO 3: INTERPRETAZIONI DELLA STATUA&#x20;

#### 5) Chi è coinvolto nella controversia aperta dal monumento? (mdo:Controversy  ceon-actor:participatingActor  ceon-actor:Stakeholder)

#### 6) Qual è l’argomentazione dello stakeholder? (ceon-actor:Stakeholder  dio:supports  mdo:Argument)

#### 7) Qual è la posizione dello stakeholder? (ceon-actor:Stakeholder  mdo:hasStance  mdo:ProRemoval/mdo:ProPreservation) manca nel modello ma c’è nei dati

#### 8) Quali valori usano gli stakeholder favorevoli alla rimozione e quelli favorevoli alla preservazione a sostegno della loro argomentazione? (per ogni proRemoval  mdo:holdsValue  mdo:Value)(per ogni proPreservation  mdo:holdsValue  mdo:Value)

#### 9) Stakeholder ProRemoval e ProPreservation hanno valori in comune nelle loro argomentazioni?

***

### LIVELLO 4 - DIBATTITO, POSIZIONI E SOLUZIONI

#### 10) Quali proteste/azioni sono legate al monumento? (tip:timeIndexedParticipation  tip:includesObject  dbo:Monument)

#### 11) Chi e partecipa alla protesta? (tip:timeIndexedParticipation  tip:forEntity  ceon-actor:Stakeholder)

#### 12) Quando partecipa alla protesta? (tip:timeIndexedParticipation  tip:atTime  tip:TimeInterval; tip:TimeInterval  time:hasBeginning  time:Instant ECCETERA)

#### 13) Dove si svolge la protesta? tip:timeIndexedParticipation  tip:isSettingFor  mdo:DebateSetting)

#### 14) Qual è l’esito del dibattito? (deo:Discussion  mdo:resultsIn  mdo:ActionProposal)

#### 15) Which statue-related contestation events were amplified through traditional or social media?

