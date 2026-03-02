---
icon: arrow-progress
---

# Methodology

The creation of the Monument Debate Ontology (MDO) followed a structured, multi-step process designed to balance conceptual clarity, cultural relevance, and semantic interoperability.

{% stepper %}
{% step %}
<mark style="color:$primary;">**Domain Analysis and Preliminary Research**</mark>

we began with an in-depth review of existing literature on controversial monuments, collective memory, heritage studies, public history, and cancel culture.
This phase allowed us to identify the main dimensions of monument controversies and to understand the actors involved.
{% endstep %}

{% step %}
<mark style="color:$primary;">**Identification of Key Words**</mark>

from the research phase, we extracted the main recurring terms and themes (e.g., monument, protest, memory, removal, preservation, argument). These were then organized into a glossary.
{% endstep %}

{% step %}
<mark style="color:$primary;">**Selection of Representative Case Studies**</mark>

we chose a set of 10 statues that best represent the spectrum of controversies (e.g., political figures, colonial symbols, problematic cultural icons). These case studies served as concrete anchors to test the ontology’s capacity to model real-world situations.
{% endstep %}

{% step %}
<mark style="color:$primary;">**Scenario Development**</mark>

for each selected statue, we developed user scenarios using LLM, to describe the debate from opposing perspectives: those advocating for removal and those supporting preservation.
{% endstep %}

{% step %}
<mark style="color:$primary;">**Conceptual Modeling**</mark>

based on the glossary and scenarios we developed a conceptual model of the domain. This evolved from a natural-language map of the Monument and its controversies, to a formal ontological model aligned with existing vocabularies and extended through MDO, and finally to an advanced structure by integrating both an Ontology Design Pattern (ODP) and the theoretical model of perspectivization.
{% endstep %}

{% step %}
<mark style="color:$primary;">**Visualization and Modeling**</mark>

we produced a graphical representation of the ontology, mapping classes and their relationships to facilitate comprehension and usability.
{% endstep %}
{% endstepper %}

{% step %}
<mark style="color:$primary;">**Dataset Creation**</mark>

we constructed our own dataset by combining information from the Contested Histories website and digital map, together with data extracted from the LLM-generated scenarios. We organized the data in structured Excel sheets, creating one table for each class in the conceptual model.
{% endstep %}
{% endstepper %}

{% step %}
<mark style="color:$primary;">**RDF Production**</mark>

transformation of tabular data into RDF triples using Python (Pandas and rdflib), with systematically designed URIs.
{% endstep %}
{% endstepper %}

{% step %}
<mark style="color:$primary;">**Ontology Formalization in Protégé**</mark>

The ontology structure was further refined and formalized in Protégé, where we defined class hierarchies, specified object and datatype properties, checked logical consistency and structured domains and ranges. This phase ensured coherence and formal correctness of the ontology.
{% endstep %}
{% endstepper %}

{% step %}
<mark style="color:$primary;">**Competency Questions and SPARQL Queries**</mark>

we then formulated competency questions to define the functional requirements of the ontology.
For each competency question, we developed corresponding SPARQL queries to test whether the ontology and dataset could successfully retrieve the intended information.
{% endstep %}
{% endstepper %}

{% step %}
<mark style="color:$primary;">**Ontology-Guided LLM Experimentation**</mark>

we used the ontology as a conditioning framework for a Large Language Model.
By providing the LLM with selected case studies and constraining it to our ontology structure, we asked it to generate knowledge graphs compliant with MDO.
{% endstep %}
{% endstepper %}
