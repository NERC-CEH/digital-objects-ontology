# Digital Objects Ontology and DCAT Application Profile for CEH

## Working notes

- Maybe change the name to Digital Assets Ontology given our new team name?

## Overview

- These two semantic assets work hand-in-hand to enable rich metadata descriptions of datasets
- Diagrams for both made with [draw.io](https://www.drawio.com/) using [Chowlk Visual Notation](https://chowlk.linkeddata.es/notation.html), plus custom notation (see [legend](/legend.png))

## The Digital Objects Ontology

- See [this folder](/ontology/)
- Four modules from external ontologies: FRAPO, SmOD, PRO and SCoRO
- Also heavily relies on PROV-O
- Integrates data model developed by Epimorphics
- Includes a handful of new predicates
- Scope: diverse research outputs (articles, datasets), the projects which allowed the creation of the outputs, the grants that funded the projects, environmental monitoring facilities, programmes, activities and networks, the people and organisations who contributed to the projects and outputs, and their roles
- This ontology is compatible with the [CEH DCAT AP](/CEH-DCAT-AP/)
- This ontology will be compatible with work being done on [modelling workflows](https://github.com/NERC-CEH/workflows-ontology) and modelling observed properties in datasets
- In the diagram below, classes that are coloured in are defined in more detail (i.e. the object properties and datatype properties that apply to them) in the CEH DCAT Application Profile.

![Diagram of the Digital Objects Ontology](/ontology/digital_objects_ontology.svg)

## The CEH DCAT Application Profile

- See [this folder](/CEH-DCAT-AP/)

![Overview diagram of the CEH DCAT Application Profile](/CEH-DCAT-AP/CEH-DCAT-AP_diagrams/dcat.svg)
