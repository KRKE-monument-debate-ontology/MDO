---
icon: file-waveform
---

# DATASET

## <mark style="color:$primary;">DATA COLLECTION</mark>&#x20;

Since ready-to-use open datasets for the domain we modelled were not available, we created one by combining information selected from our sources, in particular the Contested Histories website and digital map, and information extracted from the scenarios generated with ChatGPT.

We organized our data in Excel sheets, one for each Class in our conceptual model. We started with column names in natural language, that we mapped to the properties in our conceptual model in order to transform data in the Excel file into RDF.

The Excel and csv file can be downloaded [here](https://github.com/KRKE-monument-debate-ontology/Data_MDO/blob/main/dataset.xlsx?raw=true).;

The Python script used for the RDF production can be seen below.

<br>

## <mark style="color:$primary;">RDF PRODUCTION</mark>&#x20;

As mentioned, each table represents a Class with multiple properties associated to it in the data model. Each row in the table, thus, represents an instance of that class, whereas each column is either a datatype property or an object property and the values in the cells represent the objects in the triple, either a Literal or a URI.

URIs for classes in our ontology were designed as follows:

[https:{domain}/{path}/{concept}/{identifier}](https:{domain}/{path}/{concept}/{identifier})

starting from the URL of the turtle file in our github repository. After organization name, repository and folder, the URI includes the concept an entity belong to (historicalFigure, place, timeInterval, monument, etc.) and its identifier, retrieved from the dcterms:identifier property.

[https:github.com/KRKE-monument-debate-ontology/Data\_MDO/md-ontology/historicalFigure/historicalFigure\_colombo](https:github.com/KRKE-monument-debate-ontology/Data_MDO/md-ontology/historicalFigure/historicalFigure_colombo)

The RDF production was carried out using Python rdflib and Pandas. The resulting turtle dataset and the python script can be downloaded here.

{% embed url="https://github.com/KRKE-monument-debate-ontology/Data_MDO" %}

<br>
