---
icon: question
---

# Competency Questions

After structuring our ontology, we defined a set of **Competency Questions,** the key questions the model should be able to answer.

In ontology design, Competency Questions help define the scope of the model, test whether its structure is logically sound, and verify that it supports meaningful queries and reasoning. In other words, they ensure that the ontology is not only conceptually correct, but also practically useful.

Below, we present the Competency Questions developed for our **Monument Debate Ontology**, which models controversial situations related to 10 selected statues and the broader debate around cancel culture. The questions reflect both perspectives, those who support preserving the monuments and those who advocate their removal.

For clarity, the answer tables use simplified labels (e.g., _monument, controversy, place_) instead of technical csv dataset terms such as `monument_id, controversy_id or place_id` , making the results easier to read while maintaining the structure of the underlying data.

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

### <mark style="color:$primary;">LEVEL 1 - description of the monument</mark>

#### 1) Where, when, who, how?

{% code fullWidth="false" expandable="true" %}
```sparql
SELECT 
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

<table data-full-width="false"><thead><tr><th>monument</th><th width="153.5999755859375">historical figure</th><th width="80.79998779296875">date</th><th width="104.79998779296875">material</th><th width="232.79998779296875">location</th><th width="129.5999755859375">creator</th><th>funder</th><th width="173.5999755859375">feature</th><th width="215.20001220703125">contextual material</th><th width="176">heritage concept</th><th width="222.4000244140625">controversy</th></tr></thead><tbody><tr><td>Indro Montanelli Statue</td><td>Indro Montanelli (1909-2001)</td><td>2006</td><td>Bronze</td><td>Montanelli public gardens, Milan</td><td>Vito Tongiani</td><td>Municipality of Milan</td><td>Indro Montanelli seated while typing on his Olivetti</td><td>Engraved inscription on the pedestal reading "Indro Montanelli, Journalist"</td><td>Colonialism, Freedom of press, Pedophilia, Racism</td><td>The controversy triggered by Indro Montanelli Statue in the montanelli public gardens</td></tr></tbody></table>

***

### <mark style="color:$primary;">LEVEL 2 - Symbolic values related to the statue and the historical figure</mark>

#### 2) Why was the historical figure commemorated with a statue?

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

| historical figure               | legacy                              |
| ------------------------------- | ----------------------------------- |
| António Vieira (1608-1697)      | Advocacy; Religion and Spirituality |
| Mary Wollstonecraft (1759-1797) | Philosophy; Women's rights; Writing |

#### 3) Which values and concepts are associated with the statue?

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

#### 4) Why is the historical figure depicted in the monument considered controversial?

```sparql
SELECT DISTINCT ?historicalfigurelabel ?controversialFactlabel
WHERE {
  ?historicalFigure schema:performerIn ?controversialFact ;
                    rdfs:label ?historicalfigurelabel .

  ?controversialFact rdfs:label ?controversialFactlabel .
}
```

<table><thead><tr><th width="280.79998779296875">historical figure</th><th width="800">controversial fact</th></tr></thead><tbody><tr><td>Jean Baptiste Colbert (1619-1683)</td><td>Colbert was responsible for laying the foundations of the Code Noir, a legal text that legitimised slavery and institutionalised the domination and brutal treatment of people in the French colonies.</td></tr><tr><td>Jimmy Savile (1926-2011)</td><td>After his death, Jimmy Savile was accused of widespread sexual abuse of minors and adults, with investigations concluding that he had exploited his celebrity status to commit offences over several decades.</td></tr><tr><td>Carl Hagenbeck (1844-1913)</td><td>Hagenbeck was known for his exhibitions of people, especially from Africa, which were brought in Germany and displayed in circuses and zoos</td></tr></tbody></table>

***

### <mark style="color:$primary;">LEVEL 3 - Interpretations of the statue</mark>

#### 5) Who is involved in the controversy triggered by the monument?

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

#### 6) What is the argument (cut) that the stakeholder (conceptualiser) derives from the monument (eventuality) by applying their values (lens)?

```sparql
SELECT ?stakeholderLabel 
	(GROUP_CONCAT(DISTINCT ?valueLabel; separator=", ") AS ?values)
  (GROUP_CONCAT(DISTINCT ?argumentLabel; separator=", ") AS ?arguments)
  (GROUP_CONCAT(DISTINCT ?monumentTitle; separator=", ") AS ?monuments)
 
WHERE {
  ?stakeholder dio:supports ?argument ;
               rdfs:label ?stakeholderLabel .
   
  ?argument mdo:justifiedWithValue ?value .
  ?monument a dbo:Monument .
  ?monument dcterms:title ?monumentTitle .
  ?argument mdo:subject ?monument ;
                    rdfs:label ?argumentLabel .
 
 ?value rdfs:label ?valueLabel .
}
GROUP BY ?stakeholderLabel
```

<table><thead><tr><th width="174.4000244140625">monument</th><th width="428.60009765625">stakeholder</th><th width="257.60009765625">value</th><th width="673.60009765625">argument</th></tr></thead><tbody><tr><td>Mahatma Gandhi Statue</td><td>Professor Adomako Ampofo, the former Director of the Institute of African Studies at the University</td><td>Cultural identity, Decolonization, Social justice</td><td>The statue must be removed because of Gandhi’s racist statements toward African people and his casteist views.</td></tr><tr><td>Mahatma Gandhi Statue</td><td>Ministry of Foreign Affairs</td><td>Diplomacy, Historical memory</td><td>The statue should be preserved to commemorate the reputation Gandhi had earned during the later years of his life.</td></tr></tbody></table>

#### 7) What positions do stakeholders take in the controversy (pro-removal or pro-preservation)?

```sparql
SELECT ?stakeholderLabel ?perspectiveLabel ?activityLabel ?controversyLabel
WHERE {
  ?participation tip:forEntity ?stakeholder ;
                        tip:includesEvent ?activity ;
  	            mdo:hasStance ?perspective .
 
  ?controversy ceon-actor:participatingActor ?stakeholder ;
               a mdo:Controversy .
 
  ?stakeholder rdfs:label ?stakeholderLabel . 
  ?perspective rdfs:label ?perspectiveLabel .
  ?controversy rdfs:label ?controversyLabel .
  ?activity rdfs:label ?activityLabel .
}
```

<table><thead><tr><th width="282">stakeholder</th><th width="351.2000732421875">perspective</th><th>activity</th><th width="348.99993896484375">controversy</th></tr></thead><tbody><tr><td>Anna Mueller, history teacher</td><td>Pro Preservation perspective on Hagenbeck statue controversy</td><td></td><td>Online petition that originated from the debate about Hagenbeck's Statue in Tierpark Hagenberg in June 2020</td></tr><tr><td>Samuel Bako, community organizer of Afro-German heritage</td><td>Pro Removal perspective on Hagenbeck statue controversy</td><td></td><td>Online petition that originated from the debate about Hagenbeck's Statue in Tierpark Hagenberg in June 2020</td></tr></tbody></table>

#### 9) Do pro-removal and pro-preservation stakeholders share common values in their arguments?

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
GROUP BY ?valueLabel
```

<table><thead><tr><th width="159.20001220703125">value</th><th width="562.5999755859375">pro removal perspectives</th><th width="596">pro preservation perspectives</th></tr></thead><tbody><tr><td>Critical thinking</td><td>Pro Removal perspective on António Vieira statue controversy</td><td>Pro Preservation perspective on Jean Baptiste Colbert statue controversy, Pro Preservation perspective on Hagenbeck statue controversy</td></tr><tr><td>Cultural identity</td><td>Pro Removal perspective on Jean Baptiste Colbert statue controversy, Pro Removal perspective on Gandhi statue controversy</td><td>Pro Preservation perspective on Colombo statue controversy,<br>Pro Preservation perspective on Edward Colston statue controversy,<br>Pro Preservation perspective on Savile statue controversy,<br>Pro Preservation perspective on Stalin statue controversy,<br>Pro Preservation perspective on António Vieira statue controversy</td></tr></tbody></table>

***

### <mark style="color:$primary;">LEVEL 4 - Debate, Positions and Proposals</mark>

#### 10) Which contestation events or actions are associated with the monument?

```sparql
SELECT ?monumentID ?ActivityLabel 
WHERE {
  ?monument dcterms:identifier ?monumentID .
  ?Participation tip:includesObject ?monument .

  ?Participation tip:includesEvent ?activity .
  ?activity a crm:E7_Activity .

  ?activity rdfs:label ?ActivityLabel .
}
```

<table><thead><tr><th width="224">monument</th><th width="725.2000732421875">activity</th></tr></thead><tbody><tr><td>monument_savile</td><td>Following posthumous revelations of sexual abuse, the statue of Jimmy Savile in Glencoe was removed after public backlash, reflecting a rapid collapse of commemorative legitimacy.</td></tr></tbody></table>

#### 11) Which stakeholders participate in contestation events or protests?

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

#### 12) What is the temporal interval of the protest?

{% code expandable="true" %}
```sparql
SELECT DISTINCT 
    ?participationLabel
    ?stakeholderLabel  
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

<table><thead><tr><th width="436.6000061035156">participation</th><th width="224.5999755859375">stakeholder</th><th>begin</th><th>end</th></tr></thead><tbody><tr><td>Participation in the protest about Montanelli's statue</td><td>eritrean person</td><td>2012-02</td><td>2012-02</td></tr><tr><td>Participation in the protest about Montanelli's statue</td><td>history student</td><td>2012-02</td><td>2012-02</td></tr><tr><td>Participation in the protest about Montanelli's statue</td><td>feminist LGBTQ+</td><td>2018-01</td><td>2018-01</td></tr><tr><td>Participation in the protest about Montanelli's statue</td><td>right-wing mayor of Milan, Gabriele Albertini</td><td>2018-01</td><td>2018-01</td></tr></tbody></table>

#### 13) What is the setting of the contestation?

{% code expandable="true" %}
```sparql
SELECT DISTINCT ?MonumentName ?controversyLabel ?debateSettingLabel
WHERE {
  ?monument mdo:triggeredControversy ?controversy .
  BIND(STRAFTER(REPLACE(STR(?monument), "monument_", ""), "monument/") AS ?slug)
  ?participation tip:forEntity ?csvID .
  FILTER(CONTAINS(LCASE(STR(?csvID)), LCASE(?slug)))
  ?participation tip:isSettingFor ?debateSetting .
  ?debateSetting a mdo:DebateSetting .
  BIND(CONCAT("Monument ", UCASE(SUBSTR(?slug, 1, 1)), SUBSTR(?slug, 2)) AS ?MonumentName)
  OPTIONAL { ?controversy rdfs:label ?controversyLabel . }
  OPTIONAL { ?debateSetting rdfs:label ?debateSettingLabel . }
}
ORDER BY ?MonumentName
```
{% endcode %}

<table><thead><tr><th width="181.60003662109375">monument</th><th width="575.8001708984375">controversy</th><th width="254.800048828125">place</th></tr></thead><tbody><tr><td>Monument Stalin</td><td>The controversy triggered by the Stalin Statue in Városliget, Budapest</td><td>Budapest, Városliget</td></tr><tr><td>Monument Vieira</td><td>The controversy triggered by António Vieira in Trindade Coelho Square</td><td>Trindade Coelho Square</td></tr></tbody></table>

#### 14) What is the outcome of the debate?

```sparql
SELECT DISTINCT ?discussionLabel ?proposalLabel
WHERE {
  ?discussion mdo:resultsIn ?proposal .
  
  OPTIONAL { ?discussion rdfs:label ?discussionLabel . }
  OPTIONAL { ?proposal rdfs:label ?proposalLabel . }
}
```

<table><thead><tr><th width="545.2001342773438">discussion</th><th width="415.59991455078125">proposal</th></tr></thead><tbody><tr><td>The discussion that arises from different stakeholders' perspectives on Savile statu</td><td>Contextualization; Keeping; Removal</td></tr><tr><td>The discussion that arises from different stakeholders' perspectives on Gandhi statue controversy</td><td>Relocation</td></tr><tr><td>The discussion that arises from different stakeholders' perspectives on Wollstonecraft statue controversy</td><td>Replace the statue with a new one</td></tr></tbody></table>

#### 15) Which monuments triggered debates that gained significant resonance in online or social media environments?

{% code expandable="true" %}
```sparql
SELECT DISTINCT ?MonumentName ?controversyLabel ?MediaPlatform
WHERE {
  
  ?monument mdo:triggeredControversy ?controversy .
  
  ?participation tip:forEntity ?csvID .
  ?participation tip:isSettingFor ?debateSetting .
  ?debateSetting a mdo:DebateSetting .
  ?debateSetting rdfs:label ?MediaPlatform .

  FILTER (regex(str(?MediaPlatform), "social media|online|web|press", "i"))

  BIND(STRAFTER(REPLACE(STR(?monument), "monument_", ""), "monument/") AS ?slug)

  FILTER(CONTAINS(LCASE(STR(?csvID)), LCASE(?slug)))

  BIND(CONCAT("Monument ", UCASE(SUBSTR(?slug, 1, 1)), SUBSTR(?slug, 2)) AS ?MonumentName)
  
  OPTIONAL { ?controversy rdfs:label ?controversyLabel . }
}
ORDER BY ?MediaPlatform
```
{% endcode %}

<table><thead><tr><th width="240">monument</th><th width="630.60009765625">controversy</th><th width="195.60009765625">place</th></tr></thead><tbody><tr><td>Monument Hagenbeck</td><td>The controversy triggered by Carl Hagenbeck Statue in Tierpark Hagenbeck</td><td>Online</td></tr><tr><td>Monument Gandhi</td><td>The controversy triggered by Mahatma Gandhi Statue in the University of Ghana</td><td>Social Media</td></tr></tbody></table>
