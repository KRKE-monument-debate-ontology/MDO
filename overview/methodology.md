---
icon: arrow-progress
---

# Methodology

The creation of the Monument Debate Ontology (MDO) followed a structured, multi-step process designed to balance conceptual clarity, cultural relevance, and semantic interoperability.

1. **Preliminary Research**: we began by reviewing existing literature on controversial monuments, cultural heritage debates, and cancel culture.
2. **Identification of Key Words**: from the research phase, we extracted the main recurring terms and themes (e.g., monument, protest, memory, removal, preservation, argument). These were then organized into a glossary.
3. **Selection of Representative Case Studies**: we chose a set of 10 statues that best represent the spectrum of controversies (e.g., political figures, colonial symbols, problematic cultural icons). These case studies served as concrete anchors to test the ontology’s capacity to model real-world situations.
4. **Scenario Development**: for each selected statue, we developed user scenarios using AI, to describe the debate from opposing perspectives: those advocating for removal and those supporting preservation.
5. **Ontology Construction**: based on the glossary and scenarios, we designed the ontology by defining classes, properties, domains, and ranges. Where possible, we reused existing ontologies and vocabularies (e.g., schema:Event, skos:Concept) to enhance interoperability.
6. **Visualization and Modeling**: we produced a graphical representation of the ontology, mapping classes and their relationships to facilitate comprehension and usability.

# Methodology

The creation of the Monument Debate Ontology (MDO) followed a structured, multi-step process designed to balance conceptual clarity, cultural relevance, and semantic interoperability.

```mermaid
flowchart TD
    A[📚 Preliminary Research<br/>Review of literature on monuments, heritage debates, cancel culture]
    B[🔑 Identification of Key Words<br/>Extract recurring terms → glossary]
    C[🗿 Selection of Case Studies<br/>10 statues representing controversies]
    D[⚖️ Scenario Development<br/>AI-generated opposing perspectives]
    E[🧩 Ontology Construction<br/>Classes, properties, domains, reuse existing vocabularies]
    F[🖼️ Visualization and Modeling<br/>Graphical representation of ontology]

    A --> B --> C --> D --> E --> F
