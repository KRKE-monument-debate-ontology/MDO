---
description: (what are the questions that we are able to answer with our ontology?
icon: question
---

# Competency Questions

#### <sub>_possiamo dire che quelle che mostriamo come risposte sono una semplificazione_</sub>

### <mark style="color:$primary;">PREFIXES</mark>

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

### <mark style="color:$primary;">LEVEL 1 description of the monument</mark>

#### 1) Where, when, who, how?

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

<table data-full-width="false"><thead><tr><th>monument</th><th width="153.5999755859375">HistoricalFigure</th><th width="80.79998779296875">date</th><th width="104.79998779296875">material</th><th width="232.79998779296875">location</th><th width="129.5999755859375">creator</th><th>funder</th><th width="173.5999755859375">feature</th><th width="215.20001220703125">contextual material</th><th width="176">heritage concept</th><th width="222.4000244140625">controversy</th></tr></thead><tbody><tr><td>Indro Montanelli Statue</td><td>Indro Montanelli (1909-2001)</td><td>2006</td><td>Bronze</td><td>Montanelli public gardens, Milan</td><td>Vito Tongiani</td><td>Municipality of Milan</td><td>Indro Montanelli seated while typing on his Olivetti</td><td>Engraved inscription on the pedestal reading "Indro Montanelli, Journalist"</td><td>Colonialism, Freedom of press, Pedophilia, Racism</td><td>The controversy triggered by Indro Montanelli Statue in the montanelli public gardens</td></tr></tbody></table>

***

### <mark style="color:$primary;">LEVEL 2 - >Symbolic values related to the statue and the historical figure</mark>

#### 2) QUERY Why was the character celebrated with a statue? What were the reasons for commemorating the historical figure with a statue? (crm:E12  crm:P17  mdo:Legacy)

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

#### 3) Which values and concepts are associated with the statue? (mdo:Monument  mdo:reflectsHeritageOf  skos:Concept)

```sparql
SELECT DISTINCT ?conceptLabel ?monumenttitle
WHERE {
  ?monument a dbo:Monument ;
            mdo:reflectsHeritageOf ?concept ;
            dcterms:title ?monumenttitle .

  ?concept rdfs:label ?conceptLabel }
```

| concept          | monument                      |
| ---------------- | ----------------------------- |
| Authoritarianism | Stalin Statue in Budapest     |
| Colonialism      | Jean Baptiste Colbert Statue  |
| Racism           | Carl Hagenbeck Statue         |
| Sexism           | Mary Wollstonecraft Sculpture |

#### 4) For what reasons is the character depicted considered controversial? (dbo:HistoricalFigure  schema:performerIn  mdo:ControversialFact)

```sparql
SELECT DISTINCT ?historicalfigurelabel ?controversialFactlabel
WHERE {
  ?historicalFigure schema:performerIn ?controversialFact ;
                    rdfs:label ?historicalfigurelabel .

  ?controversialFact rdfs:label ?controversialFactlabel .
}
```

<table><thead><tr><th width="280.79998779296875">historicalFigure</th><th width="800">controversialFact</th></tr></thead><tbody><tr><td>Jean Baptiste Colbert (1619-1683)</td><td>Colbert was responsible for laying the foundations of the Code Noir, a legal text that legitimised slavery and institutionalised the domination and brutal treatment of people in the French colonies.</td></tr><tr><td>Jimmy Savile (1926-2011)</td><td>After his death, Jimmy Savile was accused of widespread sexual abuse of minors and adults, with investigations concluding that he had exploited his celebrity status to commit offences over several decades.</td></tr><tr><td>Carl Hagenbeck (1844-1913)</td><td>Hagenbeck was known for his exhibitions of people, especially from Africa, which were brought in Germany and displayed in circuses and zoos</td></tr></tbody></table>

***

### <mark style="color:$primary;">LEVEL 3: Interpretations of the statue</mark>

#### 5) Who is involved in the controversy triggered by the monument? (mdo:Controversy  ceon-actor:participatingActor  ceon-actor:Stakeholder)

```sparql
SELECT DISTINCT ?stakeholderlabel ?controversylabel
WHERE {
  ?controversy a mdo:Controversy .
  ?controversy ceon-actor:participatingActor ?stakeholder ;
               rdfs:label ?controversylabel .

  ?stakeholder rdfs:label ?stakeholderlabel .}
```

| stakeholder                                                                             | controversy                                                                     |
| --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Maya Johnson, sociology junior and leader in the school’s Diversity and Inclusion group | The controversy triggered by Cristoforo Colombo Statue at Pepperdine University |
| Marco Bellini, 1992 alumnus and Italian-American cultural leader                        | The controversy triggered by Cristoforo Colombo Statue at Pepperdine University |

#### 6) What is the stakeholder argument? (ceon-actor:Stakeholder  dio:supports  mdo:Argument)

```sparql
SELECT  ?argumentLabel ?stakeholderLabel ?controversyLabel
WHERE {
  ?controversy ceon-actor:participatingActor ?stakeholder .
  ?stakeholder dio:supports ?argument .
  ?argument rdfs:label ?argumentLabel .
  ?stakeholder rdfs:label ?stakeholderLabel .
  ?controversy rdfs:label ?controversyLabel .
}
```

<table><thead><tr><th width="431.39996337890625">controversy</th><th width="428.60009765625">stakeholder</th><th width="673.60009765625">argument</th></tr></thead><tbody><tr><td>The controversy triggered by Mahatma Gandhi Statue in the University of Ghana</td><td>Professor Adomako Ampofo, the former Director of the Institute of African Studies at the University</td><td>The statue must be removed because of Gandhi’s racist statements toward African people and his casteist views.</td></tr><tr><td>The controversy triggered by Mahatma Gandhi Statue in the University of Ghana</td><td>Ministry of Foreign Affairs</td><td>The statue should be preserved to commemorate the reputation Gandhi had earned during the later years of his life.</td></tr></tbody></table>

#### 7) What is the stakeholder's position? (ceon-actor:Stakeholder  mdo:hasStance  mdo:ProRemoval/mdo:ProPreservation) manca nel modello ma c’è nei dati

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

#### 8) What values are mobilized by stakeholders supporting either the removal or the preservation of monuments to sustain their respective arguments? Which values are employed by stakeholders in favor of removal and those in favor of preservation to support their arguments? (per ogni proRemoval  mdo:holdsValue  mdo:Value)(per ogni proPreservation  mdo:holdsValue  mdo:Value)

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |

#### 9) Do Pro-Removal and Pro-Preservation stakeholders share any common values in their arguments?

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

<table><thead><tr><th width="159.20001220703125">valueLabel</th><th width="562.5999755859375">proRemovalPerspectives</th><th width="596">proPreservationPerspectives</th></tr></thead><tbody><tr><td>Critical thinking</td><td>Pro Removal perspective on António Vieira statue controversy</td><td>Pro Preservation perspective on Jean Baptiste Colbert statue controversy, Pro Preservation perspective on Hagenbeck statue controversy</td></tr><tr><td>Cultural identity</td><td>Pro Removal perspective on Jean Baptiste Colbert statue controversy, Pro Removal perspective on Gandhi statue controversy</td><td>Pro Preservation perspective on Colombo statue controversy,<br>Pro Preservation perspective on Edward Colston statue controversy,<br>Pro Preservation perspective on Savile statue controversy,<br>Pro Preservation perspective on Stalin statue controversy,<br>Pro Preservation perspective on António Vieira statue controversy</td></tr></tbody></table>

***

### <mark style="color:$primary;">LEVEL 4 - DEBATE, POSITIONS AND SOLUTIONS</mark>

#### 10) Which protests or actions are linked to the monument? Which contestation events or actions are associated with the monument?(tip:timeIndexedParticipation  tip:includesObject  dbo:Monument)

```sparql
SELECT DISTINCT 
MANCA TIP:INCLUDESOBJECT NELLE TABELLE
```

#### 11) Who is participating in the protest? Who are the stakeholders participating in the protest? (tip:timeIndexedParticipation  tip:forEntity  ceon-actor:Stakeholder

{% code expandable="true" %}
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
{% endcode %}

<table><thead><tr><th width="411.5999755859375">participation</th><th width="553.199951171875">stakeholder</th></tr></thead><tbody><tr><td>Participation in the protest about Stalin's statue</td><td>István, factory worker</td></tr><tr><td>Participation in the protest about Stalin's statue</td><td>Katalin, history teacher</td></tr></tbody></table>

#### 12) When does the protest take place? What is the temporal interval of the protest?(tip:timeIndexedParticipation  tip:atTime  tip:TimeInterval; tip:TimeInterval  time:hasBeginning  time:Instant ECCETERA)

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

<table><thead><tr><th width="224.5999755859375">stakeholder</th><th width="436.6000061035156">participation</th><th>begin</th><th>end</th></tr></thead><tbody><tr><td>eritrean person</td><td>Participation in the protest about Montanelli's statue</td><td>2012-02</td><td>2012-02</td></tr><tr><td>history student</td><td>Participation in the protest about Montanelli's statue</td><td>2012-02</td><td>2012-02</td></tr><tr><td>feminist LGBTQ+</td><td>Participation in the protest about Montanelli's statue</td><td>2018-01</td><td>2018-01</td></tr><tr><td>right-wing mayor of Milan, Gabriele Albertini</td><td>Participation in the protest about Montanelli's statue</td><td>2018-01</td><td>2018-01</td></tr></tbody></table>

#### 13) Where does the protest occur? / What is the setting of the contestation? tip:timeIndexedParticipation  tip:isSettingFor  mdo:DebateSetting)

{% code expandable="true" %}
```sparql
SELECT DISTINCT ?NomeMonumento ?controversiaLabel ?luogoProtestaLabel
WHERE {
  ?monumento mdo:triggeredControversy ?controversia .
  BIND(STRAFTER(REPLACE(STR(?monumento), "monument_", ""), "monument/") AS ?slug)
  ?partecipazione tip:forEntity ?idNelCSV .
  FILTER(CONTAINS(LCASE(STR(?idNelCSV)), LCASE(?slug)))
  ?partecipazione tip:isSettingFor ?luogoProtesta .
  ?luogoProtesta a mdo:DebateSetting .
  BIND(CONCAT("Monument ", UCASE(SUBSTR(?slug, 1, 1)), SUBSTR(?slug, 2)) AS ?NomeMonumento)
  OPTIONAL { ?controversia rdfs:label ?controversiaLabel . }
  OPTIONAL { ?luogoProtesta rdfs:label ?luogoProtestaLabel . }
}
ORDER BY ?NomeMonumento
```
{% endcode %}

<table><thead><tr><th width="181.60003662109375">monumento</th><th width="575.8001708984375">controversy</th><th width="254.800048828125">place</th></tr></thead><tbody><tr><td>Monument Stalin</td><td>The controversy triggered by the Stalin Statue in Városliget, Budapest</td><td>Budapest, Városliget</td></tr><tr><td>Monument Vieira</td><td>The controversy triggered by António Vieira in Trindade Coelho Square</td><td>Trindade Coelho Square</td></tr></tbody></table>

#### 14) What is the outcome of the debate? (deo:Discussion  mdo:resultsIn  mdo:ActionProposal)

```sparql
SELECT DISTINCT ?discussionLabel ?proposalLabel
WHERE {
  ?discussion mdo:resultsIn ?proposal .
  
  OPTIONAL { ?discussion rdfs:label ?discussionLabel . }
  OPTIONAL { ?proposal rdfs:label ?proposalLabel . }
}
```

<table><thead><tr><th width="545.2001342773438">discussion</th><th width="415.59991455078125">proposal</th></tr></thead><tbody><tr><td>The discussion that arises from different stakeholders' perspectives on Savile statue controversy</td><td>The discussion among the stakeholders did not lead to a remedy action</td></tr><tr><td>The discussion that arises from different stakeholders' perspectives on Gandhi statue controversy</td><td>Relocation</td></tr><tr><td>The discussion that arises from different stakeholders' perspectives on Wollstonecraftstatue controversy</td><td>Replace the statue with a new one</td></tr></tbody></table>

#### 15)

```
// Some code
```

|   |   |   |
| - | - | - |
|   |   |   |
