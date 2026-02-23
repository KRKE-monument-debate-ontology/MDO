---
description: >-
  usare la nostra ontologia come condizionamento di un LLM --> dargli i casi
  studio e chiedergli di generare un knowledge graph basato sulla nostra
  ontologia
---

# Page 2

## Statue of Indro Montanelli

{% stepper %}
{% step %}
### Obiettivo

You must generate RDF/Turtle using ONLY the classes and properties listed below.\
Do not invent new classes or properties.\
Use correct RDF syntax.\
Use the prefix mdo: for the Monument Debate Ontology.
{% endstep %}

{% step %}
### User Story&#x20;

It is a bright spring Saturday in Milan. In Piazza della Repubblica, the bronze statue of Indro Montanelli, inaugurated in 2006, still stands. A police cordon surrounds it, keeping two groups of demonstrators at a careful distance: on one side, those calling for its removal; on the other, those defending its presence. \
\
🔴 Marta, 27, a contemporary history student, grips a sign reading, “No celebration for those who justified colonialism.” For her, the statue is a painful symbol in a public space. <mark style="color:$danger;background-color:$danger;">It recalls Montanelli’s statements on colonialism and his marriage to a twelve-year-old Eritrean girl</mark> during the war in Ethiopia. She feels that leaving the statue without context is a way of honoring him while erasing the victims’ perspective and normalizing values no longer acceptable. Marta hopes it will be removed and replaced with a work celebrating figures who promoted rights and equality. As she gazes at the statue, she thinks: “This isn’t about censorship. It’s about who we want as a role model today.” \
\
🟢 Across the cordon, Lorenzo, 72, a retired war correspondent, holds a small bouquet of flowers. For him, Montanelli was a <mark style="color:$success;background-color:$success;">master of journalism</mark>. He acknowledges the dark parts of his biography but believes they must be understood in their historical context and in the complexity of the man. To remove the statue, he argues, would be to rewrite history and lose a piece of the city’s memory. Lorenzo hopes instead to enrich the site with a plaque that tells both the journalistic achievements and the controversies. As he approaches, he thinks: “We aren’t better if we erase the past; we must confront it, not remove it.”\
\
The two groups watch each other warily, divided by barriers. Marta and Lorenzo cross paths briefly as the police allow demonstrators to move in small groups.\
— Marta: “I understand the value Montanelli had for you, but this statue hurts many.”\
— Lorenzo: “And I understand the pain. But I believe the solution is to explain, not tear it down.”\
\
➡️ Neither changes their mind. Yet, as they part ways, both carry a quiet awareness: behind every monument lies a debate that speaks more about the present than the past.
{% endstep %}

{% step %}
### RDF/Turtle generated&#x20;

{% code expandable="true" %}
```luau
@prefix pr: <http://www.ontologydesignpatterns.org/cp/owl/participantRole.owl> .
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dbo: <http://dbpedia.org/ontology/> .
@prefix deo: <http://purl.org/spar/deo/> .
@prefix dio: <https://w3id.org/dio#> .
@prefix mdo: <https://github.com/KRKE-monument-debate-ontology/Data_MDO/md-ontology/> .
@prefix ns1: <http://w3id.org/CEON/ontology/material/> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix tip: <http://ontologydesignpatterns.owl/cp/owl/timeindexedparticipation.owl/> .
@prefix xml: <http://www.w3.org/XML/1998/namespace> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .
@prefix time: <http://www.w3.org/2006/time#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix schema1: <http://schema.org/> .
@prefix ceon-actor: <http://w3id.org/CEON/ontology/actor/> .

mdo:MontanelliStatue a dbo:Monument ;
    rdfs:subClassOf crm:E24_Physical_Human-Made_Thing ;
    dcterms:title "Statue of Indro Montanelli"^^xsd:string ;
    dcterms:date "2006"^^xsd:gYear ;
    schema1:location mdo:PiazzaDellaRepubblica ;
    schema1:creator mdo:UnknownSculptor ;
    schema1:funder mdo:MunicipalityOfMilan ;
    crm:P62_depicts mdo:IndroMontanelli ;
    crm:P108_was_produced_by mdo:MontanelliProduction ;
    mdo:triggerdControversy mdo:MontanelliControversy ;
    mdo:reflectsHeritageOf mdo:JournalisticHeritage .

mdo:PiazzaDellaRepubblica a schema1:Place .

mdo:UnknownSculptor a crm:E39_Actor .

mdo:MunicipalityOfMilan a schema1:GovernmentOrganization .

mdo:MontanelliProduction a crm:E12_Production ;
    crm:P17_was_motivated_by mdo:JournalisticLegacy .

mdo:IndroMontanelli a mdo:HistoricalFigure ;
    mdo:hasLegacyImpact mdo:JournalisticLegacy ;
    schema1:PerformerIn mdo:ColonialMarriageFact .

mdo:JournalisticLegacy a mdo:Legacy .

mdo:JournalisticHeritage a skos:Concept .

mdo:ColonialMarriageFact a mdo:ControversialFacts .

mdo:MontanelliControversy a mdo:Controversy ;
    ceon-actor:participatingActor mdo:Marta ,
                                   mdo:Lorenzo .

mdo:Marta a ceon-actor:Stakeholder ;
    mdo:HoldsValue mdo:SocialJusticeValue ;
    schema1:knowsAbout mdo:JournalisticLegacy ,
                       mdo:ColonialMarriageFact ;
    dio:supports mdo:RemovalArgument .

mdo:Lorenzo a ceon-actor:Stakeholder ;
    mdo:HoldsValue mdo:HistoricalMemoryValue ;
    schema1:knowsAbout mdo:JournalisticLegacy ,
                       mdo:ColonialMarriageFact ;
    dio:supports mdo:ContextualisationArgument .

mdo:SocialJusticeValue a mdo:Value ;
    mdo:generates mdo:ProRemovalPerspective .

mdo:HistoricalMemoryValue a mdo:Value ;
    mdo:generates mdo:ProPreservationPerspective .

mdo:ProRemovalPerspective a mdo:ProRemoval ;
    mdo:emergesFrom mdo:PublicDiscussion .

mdo:ProPreservationPerspective a mdo:ProPreservation ;
    mdo:emergesFrom mdo:PublicDiscussion .

mdo:PublicDiscussion a deo:Discussion .

mdo:RemovalArgument a mdo:Argument ;
    dcterms:subject mdo:MontanelliStatue ,
                    mdo:ColonialMarriageFact ,
                    mdo:IndroMontanelli ;
    mdo:justifiedWithValue mdo:SocialJusticeValue ;
    mdo:resultsIn mdo:RemovalProposal .

mdo:ContextualisationArgument a mdo:Argument ;
    dcterms:subject mdo:MontanelliStatue ,
                    mdo:JournalisticLegacy ,
                    mdo:IndroMontanelli ;
    mdo:justifiedWithValue mdo:HistoricalMemoryValue ;
    mdo:resultsIn mdo:ContextualisationProposal .

mdo:RemovalProposal a mdo:ActionProposal ;
    crm:P2_has_type mdo:Removal .

mdo:ContextualisationProposal a mdo:ActionProposal ;
    crm:P2_has_type mdo:Contextualisation .
```
{% endcode %}
{% endstep %}

{% step %}
### Knowledge Graph&#x20;

<figure><img src="../.gitbook/assets/rdf-grapher.png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}
