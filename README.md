[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)

# Digital Objects Ontology

Documentation: https://nerc-ceh.github.io/digital-objects-ontology/

## Overview

- Scope: rich metadata for diverse research outputs (articles, datasets, methods, etc.) including descriptions of the projects which allowed the creation of the outputs, the grants that funded the projects, the environmental monitoring facilities, programmes and networks that provided the data, the people and organisations who contributed to the projects and outputs, and their roles.
- Four modules from external ontologies: [FRAPO](https://sparontologies.github.io/frapo/current/frapo.html), [FDRI](https://nerc-ceh.github.io/fdri-ontology/), [PRO](https://sparontologies.github.io/pro/current/pro.html) and [SCoRO](https://sparontologies.github.io/scoro/current/scoro.html).
- Also heavily relies on [PROV-O](https://www.w3.org/TR/prov-o/) and [FOAF](http://xmlns.com/foaf/spec/).
- Includes a handful of new predicates, for which I have used the namespace https://ceh.ac.uk/digital-objects-ontology (for now).
- This ontology works hand-in-hand with the [Environmental DCAT Application Profile (Env-DCAT-AP)](https://github.com/NERC-CEH/env-dcat-ap) to enable rich metadata descriptions of datasets.
- This ontology is compatible with work being done on [modelling workflows/method-type things](https://github.com/NERC-CEH/workflows-ontology) and will be compatible with work on modelling observed properties in datasets (not yet started).
- In the diagram below, classes that are coloured in are defined in more detail (i.e. the object properties and datatype properties that apply to them) elsewhere, either in the [Env-DCAT-AP](https://github.com/NERC-CEH/env-dcat-ap) or in the [Method Metadata Ontology](https://github.com/NERC-CEH/workflows-ontology/tree/main/method-metadata-ontology).
- Diagrams are made with [draw.io](https://www.drawio.com/) using [Chowlk Visual Notation](https://chowlk.linkeddata.es/notation.html), plus custom notation (see [legend](legend/legend.png)). The colour scheme is consistent across this repository, https://github.com/NERC-CEH/env-dcat-ap and https://github.com/NERC-CEH/workflows-ontology.

![Diagram of the Digital Objects Ontology](ontology/diagrams/digital_objects_ontology.svg)
