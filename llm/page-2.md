

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
### CLASSES AND PROPERTIES&#x20;

{% content-ref url="../knowledge-representation/classes-and-properties.md" %}
[classes-and-properties.md](../knowledge-representation/classes-and-properties.md)
{% endcontent-ref %}
{% endstep %}

{% step %}
### User Story&#x20;

In 2006, the Comune di Milano inaugurated a statue of Indro Montanelli in the public gardens of Milan that bear his name. Sculpted by Vito Tongiani, the bronze figure shows Montanelli seated at his typewriter. The monument was intended as a tribute to one of the most influential Italian journalists of the twentieth century. For several years, it stood largely as a symbol of journalistic achievement and civic recognition.

The first rupture came in February 2012. One morning, the statue was found splashed with red paint, and a fake bomb made of wood and paper had been placed beneath its hat. The act marked the beginning of a public controversy. Among those present that day was an <mark style="color:red;background-color:red;">Eritrean man who supported the gesture</mark>. To him, the red paint <mark style="color:red;background-color:red;">symbolized the violence of Italian colonialism</mark> and forced the city to confront a history often left unaddressed. Not far away, a <mark style="color:green;background-color:green;">history student reacted differently</mark>. He was disturbed by the vandalism and believed that damaging a monument only deepened divisions. In his view, <mark style="color:green;background-color:green;">the statue represented a piece of public memory</mark>—problematic, perhaps, but something to be studied and debated rather than attacked.

The debate intensified in 2020. During a protest organized by <mark style="color:red;background-color:red;">Rete Studenti Milano</mark>, demonstrators once again targeted the monument. They threw red paint and wrote the words “Racist, Rapist” at its base. For the students, the statue was not a neutral tribute but a public <mark style="color:red;background-color:red;">celebration that ignored Montanelli’s support for colonialism and his marriage to a twelve-year-old Eritrean girl</mark>. They argued that a city cannot honor a figure in a shared public space without acknowledging the harm connected to his actions. Some called for the statue’s removal; others demanded clear contextualization.

Across the police cordon stands a <mark style="color:green;background-color:green;">journalist</mark>, visibly shaken by the scene. He speaks to a small group of supporters, explaining that <mark style="color:green;background-color:green;">Montanelli’s legacy in journalism is immense, that he shaped Italian reporting for decades.</mark> Removing the statue, he says, would not correct history—it would erase it. He believes the monument should remain, perhaps contextualized, but not torn down.

Since then, the statue has remained in place. Yet its meaning has changed. What was inaugurated in 2006 as a straightforward tribute has become a focal point of competing memories—one centered on colonial violence and moral accountability, the other on professional legacy and the preservation of history. The bronze figure has not moved, but the story surrounding it continues to evolve.
{% endstep %}

{% step %}
### RDF/Turtle generated&#x20;

{% code expandable="true" %}
```luau
@prefix mdo: <https://github.com/KRKE-monument-debate-ontology/Data_MDO/md-ontology/> .
@prefix crm: <http://www.cidoc-crm.org/cidoc-crm/> .
@prefix dbo: <http://dbpedia.org/ontology/> .
@prefix deo: <http://purl.org/spar/deo/> .
@prefix dio: <https://w3id.org/dio#> .
@prefix ceon-actor: <http://w3id.org/CEON/ontology/actor/> .
@prefix pr: <http://www.ontologydesignpatterns.org/cp/owl/participantRole.owl> .
@prefix tip: <http://ontologydesignpatterns.owl/cp/owl/timeindexedparticipation.owl/> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix schema: <http://schema.org/> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
 
### MONUMENT AND FIGURE ###
 
mdo:Statue_Montanelli a dbo:Monument ;
    rdfs:label "Indro Montanelli Statue" ;
    dcterms:date "2006"^^xsd:gYear ;
    crm:P62 mdo:IndroMontanelli ;
    mdo:triggeredControversy mdo:Controversy_Montanelli .
 
mdo:IndroMontanelli a mdo:HistoricalFigure ;
    rdfs:label "Indro Montanelli" ;
    mdo:hasLegacyImpact mdo:Legacy_Journalism ;
    schema:performerIn mdo:Colonial_Actions .
 
mdo:Colonial_Actions a mdo:ControversialFact ;
    rdfs:label "Colonial past and marriage to minor" .
 
mdo:Legacy_Journalism a mdo:Legacy ;
    rdfs:label "Journalistic Achievement" .
 
### DISCUSSIONS ###
 
mdo:Discussion_2012 a deo:Discussion ;
    rdfs:label "2012 Public Controversy" ;
    dcterms:date "2012"^^xsd:gYear .
 
mdo:Discussion_2020 a deo:Discussion ;
    rdfs:label "2020 Public Debate" ;
    dcterms:date "2020"^^xsd:gYear .
 
### PERSPECTIVES 2012 (Emergenti dalla discussione 2012) ###
 
mdo:Perspective_Removal_2012 a mdo:ProRemoval ;
    rdfs:label "Pro-Removal Perspective (2012)" ;
    mdo:emergesFrom mdo:Discussion_2012 .
 
mdo:Perspective_Preservation_2012 a mdo:ProPreservation ;
    rdfs:label "Pro-Preservation Perspective (2012)" ;
    mdo:emergesFrom mdo:Discussion_2012 .
 
### PERSPECTIVES 2020 (Emergenti dalla discussione 2020) ###
 
mdo:Perspective_Removal_2020 a mdo:ProRemoval ;
    rdfs:label "Pro-Removal Perspective (2020)" ;
    mdo:emergesFrom mdo:Discussion_2020 .
 
mdo:Perspective_Preservation_2020 a mdo:ProPreservation ;
    rdfs:label "Pro-Preservation Perspective (2020)" ;
    mdo:emergesFrom mdo:Discussion_2020 .
 
### ACTIVITIES (crm:E7) ###
 
mdo:Activity_Vandalism_2012 a crm:E7 ;
    rdfs:label "2012 Vandalism Event" ;
    dcterms:date "2012"^^xsd:gYear .
 
mdo:Activity_Protest_2020 a crm:E7 ;
    rdfs:label "2020 Protest Event" ;
    dcterms:date "2020"^^xsd:gYear .
 
### PARTICIPATIONS 2012 ###
 
# Uomo Eritreo (2012)
mdo:Participation_2012_Eritrean a tip:TimeIndexedParticipation ;
    rdfs:label "Participation of Eritrean Man (2012)" ;
    tip:forEntity mdo:Eritrean_Man ;
    tip:hasRole mdo:Role_Witness_Protestor ;
    tip:includesEvent mdo:Activity_Vandalism_2012 ;
    tip:includesObject mdo:Statue_Montanelli .
 
# Studente di Storia (2012)
mdo:Participation_2012_HistoryStudent a tip:TimeIndexedParticipation ;
    rdfs:label "Participation of History Student (2012)" ;
    tip:forEntity mdo:History_Student ;
    tip:hasRole mdo:Role_History_Observer ;
    tip:includesEvent mdo:Activity_Vandalism_2012 ;
    tip:includesObject mdo:Statue_Montanelli .
 
### PARTICIPATIONS 2020 ###
 
# Rete Studenti (2020)
mdo:Participation_2020_Students a tip:TimeIndexedParticipation ;
    rdfs:label "Participation of Rete Studenti (2020)" ;
    tip:forEntity mdo:Rete_Studenti_Milano ;
    tip:hasRole mdo:Role_Student_Org ;
    tip:includesEvent mdo:Activity_Protest_2020 ;
    tip:includesObject mdo:Statue_Montanelli .
 
# Giornalista (2020)
mdo:Participation_2020_Journalist a tip:TimeIndexedParticipation ;
    rdfs:label "Participation of Journalist (2020)" ;
    tip:forEntity mdo:Journalist_Stakeholder ;
    tip:hasRole mdo:Role_Journalist_Professional ;
    tip:includesEvent mdo:Activity_Protest_2020 ;
    tip:includesObject mdo:Statue_Montanelli .
 
### ROLES AND STANCES ###
 
# Ruoli 2012
mdo:Role_Witness_Protestor a pr:Role ;
    rdfs:label "Witness and Protest Supporter" ;
    mdo:hasStance mdo:Perspective_Removal_2012 .
 
mdo:Role_History_Observer a pr:Role ;
    rdfs:label "History Student Observer" ;
    mdo:hasStance mdo:Perspective_Preservation_2012 .
 
# Ruoli 2020
mdo:Role_Student_Org a pr:Role ;
    mdo:hasStance mdo:Perspective_Removal_2020 .
 
mdo:Role_Journalist_Professional a pr:Role ;
    mdo:hasStance mdo:Perspective_Preservation_2020 .
 
### ARGUMENTS AND VALUES ###
 
# Lato Rimozione (Social Justice)
mdo:Value_Social_Justice a mdo:Value ;
    rdfs:label "Social Justice" ;
    mdo:generates mdo:Perspective_Removal_2012, mdo:Perspective_Removal_2020 .
 
mdo:Argument_Colonialism a mdo:Argument ;
    rdfs:label "Argument: Colonial Violence" ;
    dcterms:subject mdo:Colonial_Actions ;
    mdo:justifiedWithValue mdo:Value_Social_Justice .
 
# Lato Conservazione (Historical Memory)
mdo:Value_Historical_Memory a mdo:Value ;
    rdfs:label "Historical Memory" ;
    mdo:generates mdo:Perspective_Preservation_2012, mdo:Perspective_Preservation_2020 .
 
mdo:Argument_Legacy a mdo:Argument ;
    rdfs:label "Argument: Professional History" ;
    dcterms:subject mdo:Legacy_Journalism ;
    mdo:justifiedWithValue mdo:Value_Historical_Memory .
 
### STAKEHOLDERS (dio:supports) ###
 
mdo:Eritrean_Man a ceon-actor:Stakeholder ; dio:supports mdo:Argument_Colonialism .
mdo:Rete_Studenti_Milano a ceon-actor:Stakeholder ; dio:supports mdo:Argument_Colonialism .
mdo:History_Student a ceon-actor:Stakeholder ; dio:supports mdo:Argument_Legacy .
mdo:Journalist_Stakeholder a ceon-actor:Stakeholder ; dio:supports mdo:Argument_Legacy .
```
{% endcode %}
{% endstep %}

{% step %}
### Knowledge Graph&#x20;

<figure><img src="../.gitbook/assets/rdf-grapher (2).png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}
