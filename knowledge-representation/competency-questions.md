---
description: (what are the questions that we are able to answer with our ontology?
icon: question
---

# Competency Questions

#### <sub>_possiamo dire che quelle che mostriamo come risposte sono una semplificazione_</sub>&#x20;

### <mark style="color:$primary;">PREFIXES</mark>&#x20;

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

### <mark style="color:$primary;">LIVELLO 1 descrizione del monumento</mark>&#x20;

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

### <mark style="color:$primary;">LIVELLO 2 - valori simbolici legati alla statua e al personaggio storico</mark>

#### 2) QUERY Perché il personaggio è stato celebrato con una statua? (crm:E12  crm:P17  mdo:Legacy)

{% code fullWidth="false" expandable="true" %}
```sparql
SELECT 
  ?historicalFigureLabel 
  (GROUP_CONCAT(DISTINCT ?legacyLabel; separator="; ") AS ?Legacy)
WHERE {
  ?monument crm:P62 ?historicalFigure .
  OPTIONAL { 
    ?historicalFigure rdfs:label ?historicalFigureLabel .}
  OPTIONAL {
    ?historicalFigure mdo:hasLegacyImpact ?legacy .
    ?legacy rdfs:label ?legacyLabel .}
}
GROUP BY ?historicalFigureLabel
```
{% endcode %}

| HistoricalFigure                | Legacy                              |
| ------------------------------- | ----------------------------------- |
| António Vieira (1608-1697)      | Advocacy; Religion and Spirituality |
| Mary Wollstonecraft (1759-1797) | Philosophy; Women's rights; Writing |

#### 3) Quali valori e concetti sono associati alla statua? (mdo:Monument  mdo:reflectsHeritageOf  skos:Concept)

```sparql
SELECT DISTINCT ?conceptLabel ?monumentTitle
WHERE {
  ?monument a dbo:Monument ;
            mdo:reflectsHeritageOf ?concept ;
            dcterms:title ?monumenttitle .

  ?concept rdfs:label ?conceptLabel }
```

| conceptLabel     | monumentTtle                  |
| ---------------- | ----------------------------- |
| Authoritarianism | Stalin Statue in Budapest     |
| Colonialism      | Jean Baptiste Colbert Statue  |
| Racism           | Carl Hagenbeck Statue         |
| Sexism           | Mary Wollstonecraft Sculpture |

#### 4) Per quali motivi il personaggio raffigurato è considerato controverso ? (dbo:HistoricalFigure  schema:performerIn  mdo:ControversialFact)

```sparql
SELECT DISTINCT ?historicalfigurelabel ?controversialFactlabel
WHERE {
  ?historicalFigure schema:performerIn ?controversialFact ;
                    rdfs:label ?historicalfigurelabel .

  ?controversialFact rdfs:label ?controversialFactlabel .
}
```

<table><thead><tr><th width="280.79998779296875">historicalFigureLabel</th><th width="800">controversialFactLabel</th></tr></thead><tbody><tr><td>Jean Baptiste Colbert (1619-1683)</td><td>Colbert was responsible for laying the foundations of the Code Noir, a legal text that legitimised slavery and institutionalised the domination and brutal treatment of people in the French colonies.</td></tr><tr><td>Jimmy Savile (1926-2011)</td><td>After his death, Jimmy Savile was accused of widespread sexual abuse of minors and adults, with investigations concluding that he had exploited his celebrity status to commit offences over several decades.</td></tr><tr><td>Carl Hagenbeck (1844-1913)</td><td>Hagenbeck was known for his exhibitions of people, especially from Africa, which were brought in Germany and displayed in circuses and zoos</td></tr></tbody></table>

***

### <mark style="color:$primary;">LIVELLO 3: INTERPRETAZIONI DELLA STATUA</mark>&#x20;

#### 5) Chi è coinvolto nella controversia aperta dal monumento? (mdo:Controversy  ceon-actor:participatingActor  ceon-actor:Stakeholder)

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

#### 6) Qual è l’argomentazione dello stakeholder? (ceon-actor:Stakeholder  dio:supports  mdo:Argument)

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

#### 7) Qual è la posizione dello stakeholder? (ceon-actor:Stakeholder  mdo:hasStance  mdo:ProRemoval/mdo:ProPreservation) manca nel modello ma c’è nei dati

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

#### 8) Quali valori usano gli stakeholder favorevoli alla rimozione e quelli favorevoli alla preservazione a sostegno della loro argomentazione? (per ogni proRemoval  mdo:holdsValue  mdo:Value)(per ogni proPreservation  mdo:holdsValue  mdo:Value)

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

#### 9) Stakeholder ProRemoval e ProPreservation hanno valori in comune nelle loro argomentazioni?

{% code expandable="true" %}
```sparql
SELECT 
  ?valueLabel
  (GROUP_CONCAT(DISTINCT ?pers1Label; separator=", ") AS ?proRemovalPerspectives)
  (GROUP_CONCAT(DISTINCT ?pers2Label; separator=", ") AS ?proPreservationPerspectives)
WHERE {
  ?value mdo:generates ?pers1 .
  ?pers1 a mdo:ProRemoval .
  OPTIONAL { ?pers1 rdfs:label ?pers1Label . }
  ?value mdo:generates ?pers2 .
  ?pers2 a mdo:ProPreservation .
  OPTIONAL { ?pers2 rdfs:label ?pers2Label . }
  OPTIONAL { ?value rdfs:label ?valueLabel . }
}
GROUP BY ?value ?valueLabel
```
{% endcode %}

<table><thead><tr><th width="159.20001220703125">valueLabel</th><th width="562.5999755859375">proRemovalPerspectives</th><th width="596">proPreservationPerspectives</th></tr></thead><tbody><tr><td>Critical thinking</td><td>Pro Removal perspective on António Vieira statue controversy</td><td>Pro Preservation perspective on Jean Baptiste Colbert statue controversy, Pro Preservation perspective on Hagenbeck statue controversy</td></tr><tr><td>Cultural identity</td><td>Pro Removal perspective on Jean Baptiste Colbert statue controversy, Pro Removal perspective on Gandhi statue controversy</td><td>Pro Preservation perspective on Colombo statue controversy, <br>Pro Preservation perspective on Edward Colston statue controversy, <br>Pro Preservation perspective on Savile statue controversy, <br>Pro Preservation perspective on Stalin statue controversy, <br>Pro Preservation perspective on António Vieira statue controversy</td></tr></tbody></table>

***

### <mark style="color:$primary;">LIVELLO 4 - DIBATTITO, POSIZIONI E SOLUZIONI</mark>

#### 10) Quali proteste/azioni sono legate al monumento? (tip:timeIndexedParticipation  tip:includesObject  dbo:Monument)

```sparql
SELECT DISTINCT 
MANCA TIP:INCLUDESOBJECT NELLE TABELLE
```

#### 11) Chi e partecipa alla protesta? (tip:timeIndexedParticipation  tip:forEntity  ceon-actor:Stakeholder

```sparql
SELECT DISTINCT 
	?participationLabel
	?stakeholderLabel  

WHERE {
  ?participation a tip:TimeIndexedParticipation ;
                 tip:forEntity ?stakeholder .

  ?stakeholder a ceon-actor:Stakeholder .

  OPTIONAL { ?stakeholder rdfs:label ?stakeholderLabel . }
  OPTIONAL { ?participation rdfs:label ?participationLabel . }
}
ORDER BY ?participation
```

<table><thead><tr><th width="411.5999755859375">participationLabel</th><th width="553.199951171875">stakeholderLabel</th></tr></thead><tbody><tr><td>Participation in the protest about Gandhi's statue</td><td>Professor Adomako Ampofo, the former Director of the Institute of African Studies at the University</td></tr><tr><td>Participation in the protest about Gandhi's statue</td><td>Ministry of Foreign Affairs</td></tr></tbody></table>

#### 12) Quando partecipa alla protesta? (tip:timeIndexedParticipation  tip:atTime  tip:TimeInterval; tip:TimeInterval  time:hasBeginning  time:Instant ECCETERA)

{% code expandable="true" %}
```sparql
SELECT DISTINCT 
    ?stakeholderLabel  
    ?participationLabel
    ?beginValue
    ?endValue
WHERE {
  ?participation a tip:TimeIndexedParticipation ;
                 tip:forEntity ?stakeholder ;
                 tip:atTime ?interval .

  ?stakeholder a ceon-actor:Stakeholder .

  ?interval time:hasBeginning ?beginInstant ;
            time:hasEnd ?endInstant .

  ?beginInstant a time:Instant .
  ?endInstant a time:Instant .

  OPTIONAL { ?stakeholder rdfs:label ?stakeholderLabel . }
  OPTIONAL { ?participation rdfs:label ?participationLabel . }
  OPTIONAL { ?beginInstant time:inXSDgYearMonth ?beginValue . }
  OPTIONAL { ?endInstant time:inXSDgYearMonth ?endValue . }
}
ORDER BY ?participation
```
{% endcode %}

<table><thead><tr><th width="224.5999755859375">stakeholderLabel</th><th width="436.6000061035156">participationLabel</th><th>beginValue</th><th>endValue</th></tr></thead><tbody><tr><td>eritrean person</td><td>Participation in the protest about Montanelli's statue</td><td>2012-02</td><td>2012-02</td></tr><tr><td>history student</td><td>Participation in the protest about Montanelli's statue</td><td>2012-02</td><td>2012-02</td></tr><tr><td>feminist LGBTQ+</td><td>Participation in the protest about Montanelli's statue</td><td>2018-01</td><td>2018-01</td></tr><tr><td>right-wing mayor of Milan, Gabriele Albertini</td><td>Participation in the protest about Montanelli's statue</td><td>2018-01</td><td>2018-01</td></tr></tbody></table>

#### 13) Dove si svolge la protesta? tip:timeIndexedParticipation  tip:isSettingFor  mdo:DebateSetting)

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

#### 14) Qual è l’esito del dibattito? (deo:Discussion  mdo:resultsIn  mdo:ActionProposal)

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

#### 15) Which statue-related contestation events were amplified through traditional or social media?

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

