# Digital Objects Ontology

## Working notes

- Maybe change the name to Digital Assets Ontology given our new team name?

## Overview

- This semantic asset works hand-in-hand with the Env-DCAT-AP to enable rich metadata descriptions of datasets
- Diagrams are made with [draw.io](https://www.drawio.com/) using [Chowlk Visual Notation](https://chowlk.linkeddata.es/notation.html), plus custom notation (see [legend](/legend.png)). The colour scheme is consistent across this repository, https://github.com/NERC-CEH/env-dcat-ap and https://github.com/NERC-CEH/workflows-ontology.

## The Digital Objects Ontology

- See [this folder](/ontology/)
- Scope: diverse research outputs (articles, datasets), the projects which allowed the creation of the outputs, the grants that funded the projects, environmental monitoring facilities, programmes, activities and networks, the people and organisations who contributed to the projects and outputs, and their roles
- Four modules from external ontologies: FRAPO, SmOD, PRO and SCoRO
- Also heavily relies on PROV-O
- Integrates data model developed by Epimorphics
- Includes a handful of new predicates, for which I have used the namespace https://ceh.ac.uk/digital-objects-ontology
- This ontology is compatible with the [Env-DCAT-AP]([/CEH-DCAT-AP/](https://github.com/NERC-CEH/env-dcat-ap))
- This ontology is compatible with work being done on [modelling workflows/method-things](https://github.com/NERC-CEH/workflows-ontology) and modelling observed properties in datasets (not yet started)
- In the diagram below, classes that are coloured in are defined in more detail (i.e. the object properties and datatype properties that apply to them) elsewhere, either in the [Environmental DCAT Application Profile]([/CEH-DCAT-AP/](https://github.com/NERC-CEH/env-dcat-ap)) or in the [Workflows/Method-Thing Ontology](https://github.com/NERC-CEH/workflows-ontology).
- At some point, we should probably choose which of the many named individuals (roles) we want to keep in order to reduce the number of possibilites available (TODO)

![Diagram of the Digital Objects Ontology](ontology/diagrams/digital_objects_ontology.svg)
