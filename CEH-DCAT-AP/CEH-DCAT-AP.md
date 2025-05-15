# CEH DCAT Application Profile

Last updated: 15/05/2025

Contributors: Helen Rawsthorne

## Introduction

The EIDC holds records for datasets. Each record can be accessed in multiple formats including Turtle, which is used to express RDF data. The aim of this application profile is to provide a model that can be used to describe all resources at CEH, so as to standardise them across the organisation and align them with community best practices. It is an application profile of the [DCAT 3 vocabulary](https://www.w3.org/TR/vocab-dcat-3/). It overlaps a lot with [DCAT-AP 3.0](https://semiceu.github.io/DCAT-AP/releases/3.0.0/), but we can not use that as it is specific to data portals in the EU so is too restrictive in parts.

## Overview

The section [Main Classes](#main-classes) is dedicated to the seven main classes upon which the DCAT vocabulary is based. Each class has its own section, which details all the relevant predicates associated with it and the way they are to be used in this application profile. The section [Secondary Classes](#secondary-classes) contains details of other classes that are part of this application profile but that are not part of the DCAT vocabulary.

![Overview diagram of the main classes in the CEH DCAT Application Profile](/CEH-DCAT-AP/diagrams/dcat.svg)

Sometimes we recommend using a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes) as the object of a triple instead of explicitly instantiating the class defined as the range of a predicate, despite DCAT 3 recommending avoiding blank nodes. We do this is to avoid the unnecessary creation of new URIs when it doesn't seem useful to create an URI.

### Diagrams

The diagrams are made with [draw.io](https://www.drawio.com/) using [Chowlk Visual Notation](https://chowlk.linkeddata.es/notation.html), plus custom notation (see [legend](/legend.png)). Each one of the Main Classes has been assigned a colour, to ease interpretation of the diagrams. This colour scheme is used in a consistent manner across all diagrams in this repository.

### How to interpret the tables

#### Property column

Contains the prefixed name of the property. In the case of an opaque name, the label is given in brackets.

#### Range column

Contains the accepted range of the property. The range is either identical to the range given in the source or more strict, but never contradictory.

#### Cardinality column

Contains the accepted cardinality of the property.

- 0.. implies that usage of the property is optional unless stated otherwise in the Usage column
- 1 or 1.. implies that usage of the property is mandatory

#### Usage column

Contains usage notes for the property, which should be taken as supplementary to the definition and usage notes given in the source specification. If contradictory, use those given here. "See also" comments indicate predicates with similar semantics that should be checked to ensure correct choice is made.

#### Reuse column

Contains one or two letter codes to understand the relationship between the property and DCAT 3.

- A: reused exactly as defined and expressed in DCAT 3
- E: reused with additional usage notes or additional restrictions compared to DCAT 3 (includes cases in which the only difference is a change in the cardinality)
- N: in DCAT 3 but not in CEH-DCAT-AP
- P: CEH-DCAT-AP profile specific extension (i.e. a term not mentioned in DCAT 3), can be used in combination with A or E

### Namespaces

| Prefix | Namespace IRI |
| --- | --- |
| adms | http://www.w3.org/ns/adms# |
| dcat | http://www.w3.org/ns/dcat# |
| dcterms | http://purl.org/dc/terms/ |
| doo | https://ceh.ac.uk/digital-objects-ontology/ |
| fabio | http://purl.org/spar/fabio/ |
| foaf | http://xmlns.com/foaf/0.1/ |
| geo | http://www.opengis.net/ont/geosparql# |
| geodcatap | http://data.europa.eu/930/ |
| locn | http://www.w3.org/ns/locn# |
| mmv | https://ceh.ac.uk/method-metadata-vocabulary/ |
| odrl | http://www.w3.org/ns/odrl/2/ |
| odrs | http://schema.theodi.org/odrs# |
| org | http://www.w3.org/ns/org# |
| poso | http://purl.org/poso/ |
| prov | http://www.w3.org/ns/prov# |
| rdau | http://rdaregistry.info/Elements/u/ |
| rdfs | http://www.w3.org/2000/01/rdf-schema# |
| spdx | http://spdx.org/rdf/terms# |
| time | http://www.w3.org/2006/time# |
| vcard | http://www.w3.org/2006/vcard/ns# |
| xsd | http://www.w3.org/2001/XMLSchema# |

## Specification

### Main Classes

#### Catalog

[`dcat:Catalog`](https://www.w3.org/ns/dcat#Catalog)

This class is a sub-class of `dcat:Dataset` and of `dcat:Resource`.

Every instance of this class MUST also be an instance of the class `prov:Entity`.

![Diagram of the class Catalog](/CEH-DCAT-AP/diagrams/dcat-catalog.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:homepage` | `foaf:Document` | 1 |  | A |
| `dcat:themeTaxonomy` | `rdfs:Resource` | 1 |  | A |
| `dcat:resource` | `dcat:Resource` | 0..* |  | A |
| `dcat:dataset` | `dcat:Dataset` | 0..* | SHOULD use the [DOI](https://www.doi.org/) URI. | A |
| `dcat:service` | `dcat:DataService` | 0..* |  | A |
| `dcat:catalog` | `dcat:Catalog` | 0..* |  | A |
| `dcat:record` |  |  |  | N |

#### Cataloged Resource

[`dcat:Resource`](https://www.w3.org/ns/dcat#Resource)

This class is a super-class of `dcat:Dataset`, of `dcat:DataService`, of `dcat:Catalog` and of `dcat:DatasetSeries`.

Every instance of the class `dcat:Resource`, or one of its sub-classes, MUST also be an instance of the class `prov:Entity`.

![Diagram of the class Cataloged Resource](/CEH-DCAT-AP/diagrams/dcat-resource.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcterms:accessRights` | `dcterms:RightsStatement` | 1 | SHOULD use the URI of a term from the [Limitations on public access code list](https://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess). | E |
| `dcterms:conformsTo` | `dcterms:Standard` | 0..* | If used to specify the coordinate reference system, SHOULD use the URI of a term from the OGC CRS Registry (e.g. https://www.opengis.net/def/crs/EPSG/0/4326). | E |
| `dcat:contactPoint` | `vcard:Organization` or `vcard:Individual` | 1..* | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/9] | E |
| `dcterms:creator` | `foaf:Person` or `foaf:Organization` | 0..* | Every instance of the class `dcat:Resource` MUST have at least one `dcterms:creator` or `dcterms:contributor`. SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/10] | E |
| `dcterms:description` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | E |
| `dcterms:title` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | E |
| `dcterms:issued` | `rdfs:Literal` typed as `xsd:date` | 1 | If embargoed, MUST use date when metadata record was put online and MUST NOT use embargo end date. See also `dcterms:available`. | E |
| `dcterms:modified` | `rdfs:Literal` typed as `xsd:date` | 1 | TODO [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/22] | E |
| `dcterms:language` | `dcterms:LinguisticSystem` | 1..* | MUST use the URI of a term from the [ISO 639-1](https://id.loc.gov/vocabulary/iso639-1.html). | E |
| `dcterms:publisher` | `foaf:Person` or `foaf:Organization` | 1..* | SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/23] | E |
| `dcterms:identifier` | `rdfs:Literal` typed as `xsd:anyURL` or `xsd:string` | 1..* | SHOULD use an URL. MUST use at least the catalogue-specific PID. SHOULD use the [DOI](https://www.doi.org/) URI. | E |
| `dcat:theme` |  | ?..? | TODO - will be used for the defined list of UKCEH science topics (there is a "vocabulary" but it needs updating). This is because the scope of our catalogue is broader than INSPIRE data and we've never been particularly closely aligned with those standards (see https://github.com/NERC-CEH/digital-objects-ontology/issues/19#issuecomment-2737104171) |  |
| `dcterms:type` |  | 1 | TODO [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/24] |  |
| `dcterms:relation` |  |  |  | N |
| `dcat:qualifiedRelation` |  |  |  | N |
| `dcat:keyword` | `rdfs:Literal` typed as `xsd:anyURL` | 1..* | TODO |  |
| `dcat:landingPage` | `foaf:Document` | 1..* |  | E |
| `prov:qualifiedAttribution` |  |  |  | N |
| `dcterms:license` | `dcterms:LicenseDocument` | 1 |  | E |
| `dcterms:rights` | `dcterms:RightsStatement` | 0..1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | A |
| `dcterms:hasPart` |  |  |  | N |
| `odrl:hasPolicy` |  |  |  | N |
| `dcterms:isReferencedBy` | `dcat:resource` or `prov:Entity` or `fabio:Expression` | 0..* | SHOULD use the [DOI](https://www.doi.org/) URI. MUST be used only once per resource that has referenced the subject resource (i.e. do not use twice for the DOI and for another PID of the same resource). | E |
| `dcat:previousVersion` | `dcat:resource` | 0..1 | SHOULD use the [DOI](https://www.doi.org/) URI. | E |
| `dcat:hasVersion` |  |  |  | N |
| `dcat:hasCurrentVersion` |  |  |  | N |
| `dcterms:replaces` | `dcat:resource` | 0..* | SHOULD use the [DOI](https://www.doi.org/) URI. | E |
| `dcat:version` | `rdfs:Literal` typed as `xsd:string` | 1 | Use [SemVer](http://semver.org/) or [SchemaVer](https://snowplow.io/blog/introducing-schemaver-for-semantic-versioning-of-schemas) (where possible). |  |
| `adms:versionNotes` | `rdfs:Literal` typed as `xsd:string` | 0..1 | MUST be used if previous version exists. MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/28] | E |
| `adms:status` | `skos:Concept` | 1 | TODO [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/25] | E |
| `dcat:first` |  |  |  | N |
| `dcat:last` |  |  |  | N |
| `dcat:prev` |  |  |  | N |
| `prov:wasGeneratedBy` | `prov:Activity` | 0..* | TODO |  |
| `rdau:P60402` (has custodian) | `foaf:Person` or `foaf:Organization` | 1 | SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/26] | P E |
| `rdau:P60400` (has current owner) | `foaf:Person` or `foaf:Organization` | 1 | SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/26] | P E |
| `dcterms:contributor` | `foaf:Person` or `foaf:Organization` | 0..* | Every instance of the class `dcat:Resource` MUST have at least one `dcterms:creator` or `dcterms:contributor`. SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/10] | P E |
| `geodcatap:topicCategory` | `skos:Concept` | 1..* | MUST use the URI of a term from the [Topic categories in accordance with EN ISO 19115 code list](https://inspire.ec.europa.eu/metadata-codelist/TopicCategory). | P A |
| `dcterms:bibliographicCitation` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `dcterms:available` | `rdfs:Literal` typed as `xsd:date` | 1 | If embargoed, MUST use embargo end date and MUST NOT use date when metadata record was put online. See also `dcterms:issued`. | E |
| `dcterms:provenance` |  |  | TODO, see http://purl.org/dc/terms/provenance, it's actually for a statement of any changes in ownership and custody of the resource. |  |
| `dcterms:subject` |  | 1 | TODO, see https://github.com/NERC-CEH/digital-objects-ontology/issues/19, maybe we could create a specialisation of this predicate in our own ontology and specify the vocabs we will allow people to choose thier keywords from (dcat:theme is a subproperty of this one) |  |
| `poso:hasSRS` | `poso:SRS` | 0..* | SHOULD use the URI of a term from the OGC CRS Registry (e.g. https://www.opengis.net/def/crs/EPSG/0/4326). [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/29] | P E |
| `doo:spatialRepresentationType` | `doo:SpatialRepresentation` | 0..* |  | P A |
| `mmv:distribution` | `mmv:Distribution` | 0..* | For distributions of any catalogued resource except datasets. The predicate `dcat:distribution`, pointing to the class `dcat:Distribution`, MUST be used if the subject is a `dcat:Dataset`. | P A |
| `mmv:repositoryURL` | `rdfs:Resource` | 0..* | For dynamic data stores, e.g. Github, Gitlab, Bitbucket. | P A |
| `mmv:archiveURL` | `rdfs:Resource` | 0..* | For static data stores, e.g. Zenodo, Figshare, HAL. | P A |
| `mmv:demoURL` | `rdfs:Resource` | 0..* | For web app demos, visualisations, tutorials, etc. | P A |
| `prov:wasDerivedFrom` | `prov:Entity` | 0..* | E.g. a model can be derived from a dataset, a dataset can be derived from a method. | P A |
| `prov:wasInfluencedBy` | `prov:Entity` | 0..* | E.g. a method can be influenced by another method. | P A |

#### Catalog Record

[`dcat:CatalogRecord`](https://www.w3.org/ns/dcat#CatalogRecord)

Not used.

#### Dataset

[`dcat:Dataset`](https://www.w3.org/ns/dcat#Dataset)

This class is a sub-class of `dcat:Resource` and a super-class of `dcat:Catalog`.

Every instance of this class MUST also be an instance of the class `prov:Entity`.

![Diagram of the class Dataset](/CEH-DCAT-AP/diagrams/dcat-dataset.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:distribution` | `dcat:Distribution` | 0..* |  | A |
| `dcterms:accrualPeriodicity` |  |  |  | N |
| `dcat:inSeries` | `dcat:DatasetSeries` | 0..* | [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/11] | A |
| `dcterms:spatial` | `dcterms:Location` | 0..? | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/20] | E |
| `dcat:spatialResolutionInMeters` | `rdfs:Literal` typed as `xsd:decimal` [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/27] | 0..1 |  | A |
| `dcterms:temporal` | `dcterms:PeriodOfTime` | 0..1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/5] | E |
| `dcat:temporalResolution` | `rdfs:Literal` typed as `xsd:duration` [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/27] | 0..1 |  | A |

#### Dataset Series

[`dcat:DatasetSeries`](https://www.w3.org/ns/dcat#DatasetSeries)

This class is a sub-class of `dcat:Dataset` and of `dcat:Resource`.

Every instance of this class MUST also be an instance of the class `prov:Entity`.

![Diagram of the class Dataset Series](/CEH-DCAT-AP/diagrams/dcat-datasetseries.svg)

#### Distribution

[`dcat:Distribution`](https://www.w3.org/ns/dcat#Distribution)

![Diagram of the class Distribution](/CEH-DCAT-AP/diagrams/dcat-distribution.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcterms:title` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | E |
| `dcterms:description` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | E |
| `dcterms:issued` | `rdfs:Literal` typed as `xsd:date` | 1 | If embargoed, MUST use date when metadata record was put online and MUST NOT use embargo end date. | E |
| `dcterms:modified` | `rdfs:Literal` typed as `xsd:date` | 1 | TODO [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/22] | E |
| `dcterms:license` | `dcterms:LicenseDocument` | 1 |  | A |
| `dcterms:accessRights` | `dcterms:RightsStatement` | 1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | E |
| `dcterms:rights` | `dcterms:RightsStatement` | 0..1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | A |
| `odrl:hasPolicy` |  |  |  | N |
| `dcat:accessURL` | `rdfs:Resource` | 0..* |  | A |
| `dcat:accessService` | `dcat:DataService` | 0..* |  | A |
| `dcat:downloadURL` | `rdfs:Resource` | 0..* |  | A |
| `dcat:byteSize` | `rdfs:Literal` typed as `xsd:nonNegativeInteger` | 0..1 |  | E |
| `dcat:spatialResolutionInMeters` | `rdfs:Literal` typed as `xsd:decimal` [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/27] | 0..1 |  | E |
| `dcat:temporalResolution` | `rdfs:Literal` typed as `xsd:duration` [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/27] | 0..1 |  | A |
| `dcterms:conformsTo` | `dcterms:Standard` | 0..* | If used to specify the coordinate reference system, SHOULD use the URI of a term from the OGC CRS Registry (e.g. https://www.opengis.net/def/crs/EPSG/0/4326). | E |
| `dcat:mediaType` | `dcterms:MediaType` | ?..? | TODO | A |
| `dcterms:format` | `dcterms:MediaTypeOrExtent` | ?..? | TODO | A |
| `dcat:compressFormat` | `dcterms:MediaType` | ?..? | TODO | A |
| `dcat:packageFormat` | `dcterms:MediaType` | 0..1 |  | A |
| `poso:hasSRS` | `poso:SRS` | 0..* | SHOULD use the URI of a term from the OGC CRS Registry (e.g. https://www.opengis.net/def/crs/EPSG/0/4326). | P E |
| `doo:spatialRepresentationType` | `doo:SpatialRepresentation` | 0..* |  | P A |
| `mmv:repositoryURL` | `rdfs:Resource` | 0..* | For dynamic data stores, e.g. Github, Gitlab, Bitbucket. | P A |
| `mmv:archiveURL` | `rdfs:Resource` | 0..* | For static data stores, e.g. Zenodo, Figshare, HAL. | P A |
| `mmv:demoURL` | `rdfs:Resource` | 0..* | For web app demos, visualisations, tutorials, etc. | P A |

#### Data Service

[`dcat:DataService`](https://www.w3.org/ns/dcat#DataService)

This class is a sub-class of `dcat:Resource`.

Every instance of this class MUST also be an instance of the class `prov:Entity`.

![Diagram of the class Data Service](/CEH-DCAT-AP/diagrams/dcat-dataservice.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:endpointURL` | `rdfs:Resource` | 1 |  | A |
| `dcat:endpointDescription` | `rdfs:Resource` | 1..* |  | A |
| `dcat:servesDataset` | `dcat:Dataset` | 0..* |  | A |

### Secondary Classes

#### Rights Statement (DC Terms)

[`dcterms:RightsStatement`](http://purl.org/dc/terms/RightsStatement)

When used with predicate `dcterms:accessRights`:

![Diagram of the class Rights Statement (DC Terms)](/CEH-DCAT-AP/diagrams/dcat-dcterms-rightsstatement-accessrights.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `rdfs:label` | `rdfs:Literal` typed as `xsd:string` | 1..* | SHOULD use the label given for the resource at the resource URI. MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |

When used with predicate `dcterms:rights`:

![Diagram of the class Rights Statement (DC Terms)](/CEH-DCAT-AP/diagrams/dcat-dcterms-rightsstatement-rights.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `odrs:copyrightHolder` | `foaf:Person` or `foaf:Organization` | 0..* | SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. | P E |
| `odrs:copyrightStatement` | `foaf:Document` | 0..1 |  | P A |
| `odrs:copyrightNotice` | `rdfs:Literal` typed as `xsd:string` | 0..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `odrs:copyrightYear` | `rdfs:Literal` typed as `xsd:gYear` | 0..1 |  | P E |

#### Organization (vCard)

[`vcard:Organization`](http://www.w3.org/2006/vcard/ns#Organization)

How to define URI? Could use email address if we didn't have more than one postal address for the same email. Why do we need to give postal address?

![Diagram of the class Organization (vCard)](/CEH-DCAT-AP/diagrams/dcat-vcard-organization.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:hasFN` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:hasAddress` | `vcard:Address` | 0..1 |  | P A |
| `vcard:hasEmail` | `vcard:Email` | 1 | SHOULD use a <mailto:> URI. | P E |
| `vcard:hasUID` | `rdfs:Literal` typed as `xsd:anyURI` | 1 | SHOULD use a [ROR ID](https://ror.org/) URI. | P E |

#### Address (vCard)

[`vcard:Address`](http://www.w3.org/2006/vcard/ns#Address)

![Diagram of the class Address (vCard)](/CEH-DCAT-AP/diagrams/dcat-vcard-address.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:street-address` | `rdfs:Literal` typed as `xsd:string` | 0..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:locality` | `rdfs:Literal` typed as `xsd:string` | 0..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:region` | `rdfs:Literal` typed as `xsd:string` | 0..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:country-name` | `rdfs:Literal` typed as `xsd:string` | 0..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:postal-code` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |

#### Individual (vCard)

[`vcard:Individual`](http://www.w3.org/2006/vcard/ns#Individual)

SHOULD use an [ORCID](https://orcid.org/) URI to instantiate this class.

![Diagram of the class Individual (vCard)](/CEH-DCAT-AP/diagrams/dcat-vcard-individual.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:hasFN` | `rdfs:Literal` typed as `xsd:string` | 1..* |  | P E |
| `vcard:hasName` | `vcard:Name` | 0..1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | P A |
| `vcard:organization-name` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:hasAddress` | `vcard:Address` | 0..1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | P A |
| `vcard:hasEmail` | `vcard:Email` | 1 | SHOULD use a <mailto:> URI. | P E |
| `vcard:hasUID` | `rdfs:Literal` typed as `xsd:anyURI` | 1 | SHOULD use an [ORCID](https://orcid.org/) URI. | P E |

#### Name (vCard)

[`vcard:Name`](http://www.w3.org/2006/vcard/ns#Name)

![Diagram of the class Name (vCard)](/CEH-DCAT-AP/diagrams/dcat-vcard-name.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:family-name` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |
| `vcard:given-name` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |
| `vcard:honorific-prefix` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |

#### Person (FOAF)

[`foaf:Person`](http://xmlns.com/foaf/0.1/Person)

SHOULD use an [ORCID](https://orcid.org/) URI to instantiate this class.

![Diagram of the class Person (FOAF)](/CEH-DCAT-AP/diagrams/dcat-foaf-person.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:name` | `rdfs:Literal` typed as `xsd:string` | 1 |  | P E |
| `foaf:familyName` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |
| `foaf:givenName` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |
| `foaf:title` | `rdfs:Literal` typed as `xsd:string` | 0..1 | MUST use a language tag (e.g. `@en`). | P E |
| `org:memberOf` | `foaf:Organization` ≡ `org:Organization` | 0..* |  | P E |
| `foaf:mbox` | `owl:Thing` | 0..1 | SHOULD use a <mailto:> URI. | P E |

#### Organization (FOAF) ≡ Organization (Org)

[`foaf:Organization`](http://xmlns.com/foaf/0.1/Organization) ≡ [`org:Organization`](https://www.w3.org/ns/org#Organization)

SHOULD use a [ROR ID](https://ror.org/) URI to instantiate this class.

![Diagram of the class Organization (FOAF)](/CEH-DCAT-AP/diagrams/dcat-foaf-organization.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:name` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `foaf:mbox` | `owl:Thing` | 0..1 | SHOULD use a <mailto:> URI. | P E |
| `foaf:homepage` | `foaf:Document` | 0..1 |  | E |

#### Location (DC Terms)

[`dcterms:Location`](http://purl.org/dc/terms/Location)

![Diagram of the class Location (DC Terms)](/CEH-DCAT-AP/diagrams/dcat-dcterms-location.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `locn:geometry` |  |  |  | N |
| `dcat:bbox` | `rdfs:Literal` typed as `geo:wktLiteral` | 0..1 |  | E |
| `dcat:centroid` |  |  |  | N |

#### Period of Time (DC Terms)

[`dcterms:PeriodOfTime`](http://purl.org/dc/terms/PeriodOfTime)

![Diagram of the class Period of Time (DC Terms)](/CEH-DCAT-AP/diagrams/dcat-dcterms-periodoftime.svg)

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:startDate` | `rdfs:Literal` typed as `xsd:gYear`, `xsd:gYearMonth`, `xsd:date` or `xsd:dateTime` | 0..1 | Every instance of the class `dcterms:PeriodOfTime` MUST have at least one `dcat:startDate` or `dcat:endDate`. | E |
| `dcat:endDate` | `rdfs:Literal` typed as `xsd:gYear`, `xsd:gYearMonth`, `xsd:date` or `xsd:dateTime` | 0..1 | Every instance of the class `dcterms:PeriodOfTime` MUST have at least one `dcat:startDate` or `dcat:endDate`. | E |
| `time:hasBeginning` |  |  |  | N |
| `time:hasEnd` |  |  |  | N |

#### Activity (PROV-O)

[`prov:Activity`](https://www.w3.org/TR/prov-o/#Activity)

TODO/see methods work

 #### Distribution (Methods Metadata Vocabulary)

 `mmv:Distribution`

TODO/see methods work

#### Secondary classes not requiring definition

No predicates are used with the following classes as subjects, which is why they are not defined in this application profile.

`foaf:Document`

`dcterms:LinguisticSystem`

`dcterms:LicenseDocument`

`dcterms:Standard`

`dcterms:MediaType`

`dcterms:MediaTypeOrExtent`

`vcard:Email`

`poso:SRS`

`doo:SpatialRepresentation`

`rdfs:Resource`

`prov:Entity`
