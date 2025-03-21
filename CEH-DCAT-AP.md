# CEH DCAT Application Profile

Last updated: 20/03/2025

Contributors: Helen Rawsthorne

## Introduction

The EIDC holds records for datasets. Each record can be accessed in multiple formats including Turtle, which is used to express RDF data. The aim of this application profile is to provide a model that can be used to describe all resources at CEH, so as to standardise them across the organisation and align them with community best practices. The model is an application profile of the DCAT 3 vocabulary. It overlaps a lot with DCAT-AP 3.0, but we can not use that as it is specific to data portals in the EU so is too restrictive in parts.


## Overview

Say something about main classes vs secondary classes.

Sometimes we recommend using a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes) as the object of a triple instead of explicitly instantiating the class defined as the range of a predicate. This is to avoid the unnecessary creation of new URIs. But only when it doesn't seem useful to create an URI, cos dcat recommends avoiding blank nodes.

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

Contains any usage notes, which should be taken as supplementary to the definition and usage notes in the source specification. If contradictory, use those given here.

#### Reuse column

Contains one or two letter codes to understand the relationship between the property and DCAT 3.

- A: reused as defined and expressed in DCAT 3
- E: reused with additional usage notes or additional restrictions compared to DCAT 3 (including a change in cardinality)
- N: in DCAT 3 but not in CEH-DCAT-AP
- P: CEH-DCAT-AP profile specific extension (i.e. a term not mentioned in DCAT 3), can be used in combination with A or E

### Namespaces

| Prefix | Namespace IRI |
| --- | --- |
| adms | http://www.w3.org/ns/adms# |
| dcat | http://www.w3.org/ns/dcat# |
| dcterms | http://purl.org/dc/terms/ |
| fabio | http://purl.org/spar/fabio/ |
| foaf | http://xmlns.com/foaf/0.1/ |
| geo | http://www.opengis.net/ont/geosparql# |
| geodcatap | http://data.europa.eu/930/ |
| locn | http://www.w3.org/ns/locn# |
| odrl | http://www.w3.org/ns/odrl/2/ |
| odrs | http://schema.theodi.org/odrs# |
| prov | http://www.w3.org/ns/prov# |
| rdau | http://rdaregistry.info/Elements/u/ |
| rdfs | http://www.w3.org/2000/01/rdf-schema# |
| time | http://www.w3.org/2006/time# |
| vcard | http://www.w3.org/2006/vcard/ |
| xsd | http://www.w3.org/2001/XMLSchema# |

## Specification

### Main Classes

#### Catalog

`dcat:Catalog`

This class is a sub-class of `dcat:Dataset` and of `dcat:Resource`.

Every instance of this class MUST also be an instance of the class `prov:Entity`.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:homepage` | `foaf:Document` | 1 |  | A |
| `dcat:themeTaxonomy` | `rdfs:Resource` | 1 |  | A |
| `dcat:resource` | `dcat:Resource` | 0..* |  | A |
| `dcat:dataset` | `dcat:Dataset` | 0..* | SHOULD use the [DOI](https://www.doi.org/) URI. | A |
| `dcat:service` | `dcat:DataService` | 0..* |  | A |
| `dcat:catalog` | `dcat:Catalog` | 0..* |  | A |
| `dcat:record` | `dcat:CatalogRecord` |  |  | N |

#### Cataloged Resource

`dcat:Resource`

This class is a super-class of `dcat:Dataset`, of `dcat:DataService` and of `dcat:Catalog`.

Every instance of the class `dcat:Resource`, or one of its sub-classes, MUST also be an instance of the class `prov:Entity`. This is because some properties present in this application profile have the class `prov:Entity` as their domain (e.g. `prov:qualifiedAttribution`, `prov:wasGeneratedBy`).

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcterms:accessRights` | `dcterms:RightsStatement` | 1 | SHOULD use the URI of a term from the [Limitations on public access code list](https://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess). | E |
| `dcterms:conformsTo` |  | .. | TODO |  |
| `dcat:contactPoint` | `vcard:Organization` or `vcard:Individual` | 1..* | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/9] | E |
| `dcterms:creator` | `foaf:Person` or `foaf:Organization` | 0..* | Every instance of the class `dcat:Resource` MUST have at least one `dcterms:creator` or `dcterms:contributor`. SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/10] | E |
| `dcterms:description` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | E |
| `dcterms:title` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | E |
| `dcterms:issued` | `rdfs:Literal` typed as `xsd:date` | 1 | If embargoed, MUST use date when metadata record was put online and MUST NOT use embargo end date. | E |
| `dcterms:modified` | `rdfs:Literal` typed as `xsd:date` | 1 | TODO [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/22] | E |
| `dcterms:language` | `dcterms:LinguisticSystem` | 1..* | MUST use the URI of a term from the [ISO 639-1](https://id.loc.gov/vocabulary/iso639-1.html). | E |
| `dcterms:publisher` | `foaf:Person` or `foaf:Organization` | 1..* | SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/23] | E |
| `dcterms:identifier` | `rdfs:Literal` typed as `xsd:anyURL` or `xsd:string` | 1..* | SHOULD use an URL. MUST use at least the catalogue-specific PID. SHOULD use the [DOI](https://www.doi.org/) URI. | E |
| `dcat:theme` |  | .. | TODO - will be used for the defined list of UKCEH science topics (there is a "vocabulary" but it needs updating). This is because the scope of our catalogue is broader than INSPIRE data and we've never been particularly closely aligned with those standards (see https://github.com/NERC-CEH/digital-objects-ontology/issues/19#issuecomment-2737104171) |  |
| `dcterms:type` |  | 1 | TODO [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/24] |  |
| `dcterms:relation` |  |  |  | N |
| `dcat:qualifiedRelation` |  |  |  | N |
| `dcat:keyword` |  | 0..* | TODO |  |
| `dcat:landingPage` | `foaf:Document` | 1..* |  | E |
| `prov:qualifiedAttribution` |  |  |  | N |
| `dcterms:license` | `dcterms:LicenseDocument` | 1 |  | E |
| `dcterms:rights` | `dcterms:RightsStatement` | 0..1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | A |
| `dcterms:hasPart` |  |  |  | N |
| `odrl:hasPolicy` |  |  |  | N |
| `dcterms:isReferencedBy` | `dcat:resource` or `prov:Entity` or `fabio:Expression` | 0..* | SHOULD use the [DOI](https://www.doi.org/) URI. Use only once per resource that has referenced the subject resource (i.e. do not use twice for the DOI and for the PID of the same resource). | E |
| `dcat:previousVersion` | `dcat:resource` | 0..1 | SHOULD use the [DOI](https://www.doi.org/) URI. May be used without using `dcterms:replaces`. | E |
| `dcat:hasVersion` |  |  |  | N |
| `dcat:hasCurrentVersion` |  |  |  | N |
| `dcterms:replaces` | `dcat:resource` | 0..1 | SHOULD use the [DOI](https://www.doi.org/) URI. If used, `dcat:previousVersion` must also be used. | E |
| `dcat:version` | `rdfs:Literal` typed as `xsd:string` | 1 | Use [SemVer](http://semver.org/) or [SchemaVer](https://snowplow.io/blog/introducing-schemaver-for-semantic-versioning-of-schemas) (where possible). |  |
| `adms:versionNotes` | `rdfs:Literal` typed as `xsd:string` | 0..1 | Must use if previous version exists. MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | E |
| `adms:status` |  | .. | TODO [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/25] | E |
| `dcat:first` |  |  |  | N |
| `dcat:last` |  |  |  | N |
| `dcat:prev` |  |  |  | N |
| `rdau:P60402` (has oustodian) | `foaf:Person` or `foaf:Organization` | 1 | SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/26] | P E |
| `rdau:P60400` (has current owner) | `foaf:Person` or `foaf:Organization` | 1 | SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/26] | P E |
| `dcterms:contributor` | `foaf:Person` or `foaf:Organization` | 1..* | Every instance of the class `dcat:Resource` MUST have at least one `dcterms:creator` or `dcterms:contributor`. SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/10] | P E |
| `geodcatap:topicCategory` |  | 1..* | MUST use the URI of a term from the [Topic categories in accordance with EN ISO 19115 code list](https://inspire.ec.europa.eu/metadata-codelist/TopicCategory). | P A |

[Issue: https://github.com/NERC-CEH/digital-objects-ontology/issues/22]

#### Catalog Record

`dcat:CatalogRecord`

Not used.

#### Dataset

`dcat:Dataset`

This class is a sub-class of `dcat:Resource` and a super-class of `dcat:Catalog`.

Every instance of this class MUST also be an instance of the class `prov:Entity`.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:distribution` | `dcat:Distribution` | 0..1 |  | A |
| `dcterms:accrualPeriodicity` |  |  |  | N |
| `dcat:inSeries` | `dcat:DatasetSeries` | 0..* |  | A |
| `dcterms:spatial` | `dcterms:Location` | 0.. | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | E |
| `dcat:spatialResolutionInMeters` | `rdfs:Literal` typed as `xsd:decimal` | 0..1 |  | A |
| `dcterms:temporal` | `dcterms:PeriodOfTime` | 0..1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | E |
| `dcat:temporalResolution` | `rdfs:Literal` typed as `xsd:duration` | 0..1 |  | A |
| `prov:wasGeneratedBy` | `prov:Activity` | .. | TODO |  |

#### Dataset Series

`dcat:DatasetSeries`

This class is a sub-class of `dcat:Dataset` and of `dcat:Resource`.

Every instance of this class MUST also be an instance of the class `prov:Entity`.

#### Distribution

`dcat:Distribution`

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
| `dcat:spatialResolutionInMeters` | `rdfs:Literal` typed as `xsd:decimal` | 0..1 |  | E |
| `dcat:temporalResolution` | `rdfs:Literal` typed as `xsd:duration` | 0..1 |  | A |
| `dcterms:conformsTo` | `dcterms:Standard` | 0..* |  | A |
| `dcat:mediaType` | `dcterms:MediaType` | .. | TODO | A |
| `dcterms:format` | `dcterms:MediaTypeOrExtent` | .. | TODO | A |
| `dcat:compressFormat` | `dcterms:MediaType` | .. | TODO | A |
| `dcat:packageFormat` | `dcterms:MediaType` | 0..1 |  | A |
| `spdx:checksum` | `spdx:Checksum` | 0..1 |  | A |

#### Data Service

`dcat:DataService`

This class is a sub-class of `dcat:Resource`.

Every instance of this class MUST also be an instance of the class `prov:Entity`.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:endpointURL` | `rdfs:Resource` | 1 |  | A |
| `dcat:endpointDescription` | `rdfs:Resource` | 1..* |  | A |
| `dcat:servesDataset` | `dcat:Dataset` | 0..* |  | A |

### Secondary Classes

#### Rights Statement

`dcterms:RightsStatement`

When used with predicate `dcterms:accessRights`:

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `rdfs:label` | `rdfs:Literal` typed as `xsd:string` | 1..* | SHOULD use the label given for the resource at the resource URI. MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |

When used with predicate `dcterms:rights`:

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `odrs:copyrightHolder` | `foaf:Person` or `foaf:Organization` | 0..* | SHOULD use an [ORCID](https://orcid.org/) URI for people and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. | P E |
| `odrs:copyrightStatement` | `foaf:Document` | 0..1 |  | P A |
| `odrs:copyrightNotice` | `rdfs:Literal` typed as `xsd:string` | 0..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `odrs:copyrightYear` | `rdfs:Literal` typed as `xsd:gYear` | 0..1 |  | P E |

#### Organization (vCard)

`vcard:Organization`

How to define URI? Could use email address if we didn't have more than one postal address for the same email. Why do we need to give postal address?

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:hasFN` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:hasAddress` | `vcard:Address` | 1 |  | P A |
| `vcard:hasEmail` | `vcard:Email` | 1 | SHOULD use a <mailto:> URI. | P E |
| `vcard:hasUID` | `rdfs:Literal` typed as `xsd:anyURI` | 1 | SHOULD use a [ROR ID](https://ror.org/) URI. | P E |

#### Address

`vcard:Address`

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:street-address` | `rdfs:Literal` typed as `xsd:string` | 0..1 | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:locality` | `rdfs:Literal` typed as `xsd:string` | 0..1 | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:region` | `rdfs:Literal` typed as `xsd:string` | 0..1 | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:country-name` | `rdfs:Literal` typed as `xsd:string` | 0..1 | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:postal-code` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |

#### Individual

`vcard:Individual`

SHOULD use an [ORCID](https://orcid.org/) URI to instantiate this class.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:hasFN` | `rdfs:Literal` typed as `xsd:string` | 1..* |  | P E |
| `vcard:hasName` | `vcard:Name` | 0..1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | P A |
| `vcard:organization-name` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `vcard:hasAddress` | `vcard:Address` | 0..1 | SHOULD use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | P A |
| `vcard:hasEmail` | `vcard:Email` | 1 | SHOULD use a <mailto:> URI. | P E |
| `vcard:hasUID` | `rdfs:Literal` typed as `xsd:anyURI` | 1 | ORCID | P E |

#### Name

`vcard:Name`

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:family-name` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |
| `vcard:given-name` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |
| `vcard:honorific-prefix` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |

#### Person

`foaf:Person`

SHOULD use an [ORCID](https://orcid.org/) URI to instantiate this class.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:name` | `rdfs:Literal` typed as `xsd:string` | 1 |  | P E |
| `foaf:familyName` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |
| `foaf:givenName` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |
| `foaf:title` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |
| `org:memberOf` | `foaf:Organization` ≡ `org:Organization` | 0..* |  | P E |
| `foaf:mbox` | `owl:Thing` | 0..1 | SHOULD use a <mailto:> URI. | P E |

#### Organization (FOAF) ≡ Organization (Org)

`foaf:Organization` ≡ `org:Organization`

SHOULD use a [ROR ID](https://ror.org/) URI to instantiate this class.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:name` | `rdfs:Literal` typed as `xsd:string` | 1..* | MUST use a language tag (e.g. `@en`). SHOULD be duplicated in multiple languages. | P E |
| `foaf:mbox` | `owl:Thing` | 0..1 | SHOULD use a <mailto:> URI. | P E |

#### Location

`dcterms:Location`

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `locn:geometry` |  |  |  | N |
| `dcat:bbox` | `rdfs:Literal` typed as `geo:wktLiteral` | 0..1 |  | E |
| `dcat:centroid` |  |  |  | N |

#### Period of Time

`dcterms:PeriodOfTime`

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:startDate` | `rdfs:Literal` typed as `xsd:gYear`, `xsd:gYearMonth`, `xsd:date` or `xsd:dateTime` | 0..1 | Every instance of the class `dcterms:PeriodOfTime` MUST have at least one `dcat:startDate` or `dcat:endDate`. | E |
| `dcat:endDate` | `rdfs:Literal` typed as `xsd:gYear`, `xsd:gYearMonth`, `xsd:date` or `xsd:dateTime` | 0..1 | Every instance of the class `dcterms:PeriodOfTime` MUST have at least one `dcat:startDate` or `dcat:endDate`. | E |
| `time:hasBeginning` |  |  |  | N |
| `time:hasEnd` |  |  |  | N |

#### Activity

`prov:Activity`

#### Classes not used

No predicates are used with the following classes as subjects.

`foaf:Document`

`dcterms:LinguisticSystem`

`dcterms:LicenseDocument`

`dcterms:Standard`

`dcterms:MediaType`

`dcterms:MediaTypeOrExtent`

`spdx:Checksum`
