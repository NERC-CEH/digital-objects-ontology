# CEH DCAT Application Profile

Last updated: 13/03/2025

Contributors: Helen Rawsthorne

## Introduction

The EIDC holds records for datasets. Each record can be accessed in multiple formats including Turtle, which is used to express RDF data. The aim of this application profile is to provide a model that can be used to describe all resources at CEH, so as to standardise them across the organisation and align them with community best practices. The model is an application profile of the DCAT 3 vocabulary. It overlaps a lot with DCAT-AP 3.0, but we can not use that as it is specific to data portals in the EU so is too restrictive in parts.


## Overview

Say something about main classes vs secondary classes.

Sometimes we recommend using a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes) as the object of a triple instead of explicitly instantiating the class defined as the range of a predicate. This is to avoid the unnecessary creation of new URIs. But only when it doesn't seem useful to create an URI, cos dcat recommends avoiding blank nodes.

### How to interpret the tables

#### Range column

Either identical to source or more strict, but never contradictory.

#### Cardinality column

0.. implies optional unless stated otherwise in Usage column

1 or 1.. implies mandatory

#### Usage column

Any usage notes written in this column should be taken as supplementary to the definition and usage notes in the source specification. If contradictory, use those given here.

#### Reuse column

To understand relationship with DCAT 3

- A: reused as defined and expressed in DCAT 3
- E: reused with additional usage notes or additional restrictions compared to DCAT 3
- N: in DCAT 3 but not in CEH-DCAT-AP
- P: CEH-DCAT-AP profile specific extension (i.e. a term not mentioned in DCAT 3), can be used in combination with A or E

### Namespaces

| Prefix | Namespace IRI |
| --- | --- |
| adms | http://www.w3.org/ns/adms# |
| dcat | http://www.w3.org/ns/dcat# |
| dcterms | http://purl.org/dc/terms/ |
| foaf | http://xmlns.com/foaf/0.1/ |
| geo | http://www.opengis.net/ont/geosparql# |
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

Every instance of this class must also be an instance of `prov:Entity`.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:homepage` | `foaf:Document` | 1 | Use the URI of the homepage, which will be inferred as an instance of the `foaf:Document` class. | A |
| `dcat:themeTaxonomy` | `rdfs:Resource` |  | TODO | E |
| `dcat:resource` | `dcat:Resource` | 0..* |  | A |
| `dcat:dataset` | `dcat:Dataset` | 0..* | Must use the [DOI](https://www.doi.org/) URI (where possible). | A |
| `dcat:service` | `dcat:DataService` | 0..* |  | A |
| `dcat:catalog` | `dcat:Catalog` | 0..* |  | A |
| `dcat:record` | `dcat:CatalogRecord` |  |  | N |

#### Cataloged Resource

`dcat:Resource`

This class is a super-class of `dcat:Dataset`, of `dcat:DataService` and of `dcat:Catalog`.

Every instance of the class `dcat:Resource`, or one of its sub-classes, must also be an instance of `prov:Entity`. This is because some properties present in this application profile have `prov:Entity` as their domain (e.g. `prov:qualifiedAttribution`, `prov:wasGeneratedBy`).

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcterms:accessRights` | `dcterms:RightsStatement` | 1 | Use the URI of a term from the [Limitations on public access code list](https://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess), which will be inferred as an instance of the `dcterms:RightsStatement` class. [Issue: or do we need a text label to go with it as in https://github.com/NERC-CEH/digital-objects-ontology/issues/21 ?] | E |
| `dcterms:conformsTo` |  | .. | TODO |  |
| `dcat:contactPoint` | `vcard:Organization` or `vcard:Individual` | 1..* | Use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | E |
| `dcterms:creator` | `foaf:Person` or `foaf:Organization` | 0..* | Every instance of the `dcat:Resource` class must have at least one `dcterms:creator` or `dcterms:contributor`. Must use an [ORCID](https://orcid.org/) URI for people (where possible) and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: what URI for people who don't/can't have an orcid?] | E |
| `dcterms:description` | `rdfs:Literal` typed as `xsd:string` | 1..* | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | E |
| `dcterms:title` | `rdfs:Literal` typed as `xsd:string` | 1..* | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | E |
| `dcterms:issued` | `rdfs:Literal` typed as `xsd:date` | 1 | If embargoed, use the date when the metadata record was put online, not the date of the end of the embargo. | E |
| `dcterms:modified` | `rdfs:Literal` typed as `xsd:date` | 1 | TODO [Issue: DCAT says this indicates a change to the resource, not just the catalog record... do we need this? shouldn't this be a new version? think about this] | E |
| `dcterms:language` | `dcterms:LinguisticSystem` | 1..* | Use the URI of a term from the [ISO 639-1](https://id.loc.gov/vocabulary/iso639-1.html), which will be inferred as an instance of the `dcterms:LinguisticSystem` class. | E |
| `dcterms:publisher` | `foaf:Person` or `foaf:Organization` | 1..* | Must use an [ORCID](https://orcid.org/) URI for people (where possible) and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: Will probably always be a `foaf:Organization`?] [Issue: can there be more than one publisher?] | E |
| `dcterms:identifier` | `rdfs:Literal` typed as `xsd:anyURL` or `xsd:string` | 1..* | Must use an URI (where possible). Must use at least the catalogue-specific PID. Must use the [DOI](https://www.doi.org/) URI (where possible). | E |
| `dcat:theme` |  | .. | TODO |  |
| `dcterms:type` |  | .. | TODO [Issue: currently we use terms from https://www.dublincore.org/specifications/dublin-core/dcmi-terms/#section-7, but maybe we should migrate to https://schema.datacite.org/meta/kernel-4.1/include/datacite-resourceType-v4.1.xsd as it includes `Workflow` and `Model`, neither of which are in dcterms? If we do that, do not include term `Other`. Both vocabs are recommended by DCAT for this predicate.] |  |
| `dcterms:relation` |  |  |  | N |
| `dcat:qualifiedRelation` |  |  |  | N |
| `dcat:keyword` |  | .. | TODO |  |
| `dcat:landingPage` | `foaf:Document` | 1..* | Use the URI of the landing page, which will be inferred as an instance of the `foaf:Document` class. [Issue: does it make sense to allow putting two links that direct to the same page? E.g. DOI and catalog PID.] | E |
| `prov:qualifiedAttribution` |  |  |  | N |
| `dcterms:license` | `dcterms:LicenseDocument` | 1 | Use the URI of a licence document, which will be inferred as an instance of the `dcterms:LicenseDocument` class. | E |
| `dcterms:rights` | `dcterms:RightsStatement` | 0..1 | Use a [blankNodePropertyList](https://www.w3.org/TR/rdf12-turtle/#unlabeled-bnodes). | A |
| `dcterms:hasPart` |  |  |  | N |
| `odrl:hasPolicy` |  |  |  | N |
| `dcterms:isReferencedBy` | `rdfs:Literal` typed as `xsd:anyURL` or `xsd:string` | 0..* | Must use an URI (where possible). Must use the [DOI](https://www.doi.org/) URI (where possible). Use only once per resource that has referenced the subject resource (i.e. do not use twice for the DOI and for the PID of the same resource). | E |
| `dcat:previousVersion` | `rdfs:Literal` typed as `xsd:anyURL` or `xsd:string` | 0..1 | Must use an URI (where possible). Must use the [DOI](https://www.doi.org/) URI (where possible). May be used without using `dcterms:replaces`. | E |
| `dcat:hasVersion` |  |  |  | N |
| `dcat:hasCurrentVersion` |  |  |  | N |
| `dcterms:replaces` | `rdfs:Literal` typed as `xsd:anyURL` or `xsd:string` | 0..1 | Must use an URI (where possible). Must use the [DOI](https://www.doi.org/) URI (where possible). If used, `dcat:previousVersion` must also be used. | E |
| `dcat:version` | `rdfs:Literal` typed as `xsd:string` | 1 | Use [SemVer](http://semver.org/) or [SchemaVer](https://snowplow.io/blog/introducing-schemaver-for-semantic-versioning-of-schemas) (where possible). |  |
| `adms:versionNotes` | `rdfs:Literal` typed as `xsd:string` | 0..1 | Must use if previous version exists. Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | E |
| `adms:status` |  | .. | TODO [Issue: choose a vocabulary to use out of: Those defined in the ISO standard for item registration [ISO-19135] (accepted / not accepted, deprecated, experimental, reserved, retired, stable, submitted, superseded, valid / invalid). The progress codes defined in [ISO-19115] (accepted, completed, deprecated, final, historical archive, not accepted, obsolete, ongoing, pending, planned, proposed, required, retired, superseded, tentative, under development, valid, withdrawn). The ADMS Status vocabulary [ADMS-SKOS], used in [DCAT-AP], which includes four statuses: completed, deprecated, under development, and withdrawn. The dataset statuses [EUV-DS] and concept statuses [EUV-CS] vocabularies from the EU Vocabularies registry.] | E |
| `dcat:first` |  |  |  | N |
| `dcat:last` |  |  |  | N |
| `dcat:prev` |  |  |  | N |
| `rdau:P60402` | `foaf:Person` or `foaf:Organization` | 1 | Must use an [ORCID](https://orcid.org/) URI for people (where possible) and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: Will probably always be a `foaf:Organization`?] [Issue: can there be more than one custodian?] | P E |
| `rdau:P60404` | `foaf:Person` or `foaf:Organization` | 1 | Must use an [ORCID](https://orcid.org/) URI for people (where possible) and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: Will probably always be a `foaf:Organization`?] [Issue: can there be more than one owner?] [Issue: alternative would be rdau:P60400 "has current owner"] | P E |
| `dcterms:contributor` | `foaf:Person` or `foaf:Organization` | 1..* | Must have at least one instance of `dcterms:contributor` or `dcterms:creator`. Must use an [ORCID](https://orcid.org/) URI for people (where possible) and a [ROR ID](https://ror.org/) URI for organisations. If the organisation does not yet exist in ROR, submit a request to add the organisation. [Issue: what URI for people who don't/can't have an orcid?] | P E |

[Issue: how to indicate last time metadata record updated?]

#### Catalog Record

`dcat:CatalogRecord`

Not used.

#### Dataset

`dcat:Dataset`

This class is a sub-class of `dcat:Resource` and a super-class of `dcat:Catalog`.

Every instance of this class must also be an instance of `prov:Entity`.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:distribution` | `dcat:Distribution` | 0..1 |  | A |
| `dcterms:accrualPeriodicity` |  |  |  | N |
| `dcat:inSeries` | `dcat:DatasetSeries` | 0..* |  | A |
| `dcterms:spatial` | `dcterms:Location` | 0.. | Use `dcat:bbox`. [Issue: can there be more than one?] | E |
| `dcat:spatialResolutionInMeters` | `rdfs:Literal` typed as `xsd:decimal` | 0..1 |  | A |
| `dcterms:temporal` | `dcterms:PeriodOfTime` | 0..1 | Use `dcat:startDate` and `dcat:endDate` if known (can use just one of the two if the other is unknown). | E |
| `dcat:temporalResolution` | `rdfs:Literal` typed as `xsd:duration` | 0..1 |  | A |
| `prov:wasGeneratedBy` | `prov:Activity` | .. | TODO |  |

#### Dataset Series

`dcat:DatasetSeries`

This class is a sub-class of `dcat:Dataset` and of `dcat:Resource`.

Every instance of this class must also be an instance of `prov:Entity`.

#### Distribution

`dcat:Distribution`

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcterms:title` | `rdfs:Literal` typed as `xsd:string` | 1..* | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | E |
| `dcterms:description` | `rdfs:Literal` typed as `xsd:string` | 1..* | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | E |
| `dcterms:issued` | `rdfs:Literal` typed as `xsd:date` | 1 | Mandatory. If embargoed, use date when metadata record put online, not date of end of embargo. | E |
| `dcterms:modified` | `rdfs:Literal` typed as `xsd:date` | .. | [Issue: DCAT says this indicates a change to the resource, not just the catalog record... shouldn't this be a new version? think about this] | E |
| `dcterms:license` | `dcterms:LicenseDocument` | 1 | Link directly to IRI. | E |
| `dcterms:accessRights` | `dcterms:RightsStatement` | 1 | Choose a term from following vocab: https://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess. | E |
| `dcterms:rights` | `dcterms:RightsStatement` | 0..1 |  | A |
| `odrl:hasPolicy` |  |  |  | N |
| `dcat:accessURL` | `rdfs:Resource` | 0..* |  | A |
| `dcat:accessService` | `dcat:DataService` | 0..* |  | A |
| `dcat:downloadURL` | `rdfs:Resource` | 0..* |  | A |
| `dcat:byteSize` | `rdfs:Literal` typed as `xsd:nonNegativeInteger` | 0..1 |  | E |
| `dcat:spatialResolutionInMeters` | `rdfs:Literal` typed as `xsd:decimal` | 0..1 |  | E |
| `dcat:temporalResolution` | `rdfs:Literal` typed as `xsd:duration` | 0..1 |  | A |
| `dcterms:conformsTo` | `dcterms:Standard` | 0..* |  | A |
| `dcat:mediaType` | `dcterms:MediaType` | .. |  | A |
| `dcterms:format` | `dcterms:MediaTypeOrExtent` | .. |  | A |
| `dcat:compressFormat` | `dcterms:MediaType` | .. |  | A |
| `dcat:packageFormat` | `dcterms:MediaType` | 0..1 |  | A |
| `spdx:checksum` | `spdx:Checksum` | 0..1 |  | A |

#### Data Service

`dcat:DataService`

This class is a sub-class of `dcat:Resource`.

Every instance of this class must also be an instance of `prov:Entity`.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:endpointURL` | `rdfs:Resource` | 1 |  | A |
| `dcat:endpointDescription` | `rdfs:Resource` | 1..* |  | A |
| `dcat:servesDataset` | `dcat:Dataset` | 0..* |  | A |

### Secondary Classes

#### Rights Statement

`dcterms:RightsStatement`

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `odrs:copyrightHolder` | `foaf:Person` or `foaf:Organization` | .. | Wherever possible, use ORCID for individuals and ROR for organisations. If ROR doesn't exist, submit a request to add the organisation. [Issue: Will probably always be a `foaf:Organization`?] [Issue: is it possible for there to be more than one?] | P E |
| `odrs:copyrightStatement` | `foaf:Document` | 0..1 |  | P A |
| `odrs:copyrightNotice` | `rdfs:Literal` typed as `xsd:string` | 0..* | Use when predicate is `dcterms:rights`. Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | P E |
| `odrs:copyrightYear` | `rdfs:Literal` typed as `xsd:gYear` | 0..1 |  | P E |

#### Organization (vCard)

`vcard:Organization`

How to define URI? Could use email address if we didn't have more than one postal address for the same email. Why do we need to give postal address?

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:hasFN` | `rdfs:Literal` typed as `xsd:string` | 1..* | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | P E |
| `vcard:hasAddress` | `vcard:Address` | 1 |  | P A |
| `vcard:hasEmail` | `vcard:Email` | 1 | Use <mailto:> | P E |
| `vcard:hasUID` | `rdfs:Literal` typed as `xsd:anyURI` | 1 | ROR | P E |

#### Individual

`vcard:Individual`

Use <mailto:> as URI.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:hasFN` | `rdfs:Literal` typed as `xsd:string` | 1..* | [Issue: consider breaking down into title, given name, family name?] | P E |
| `vcard:organization-name` | `rdfs:Literal` typed as `xsd:string` | 1..* | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | P E |
| `vcard:hasAddress` | `vcard:Address` | 1 |  | P A |
| `vcard:hasEmail` | `vcard:Email` | 1 | Use <mailto:> | P E |
| `vcard:hasUID` | `rdfs:Literal` typed as `xsd:anyURI` | 1 | ORCID | P E |

#### Address

`vcard:Address`

Blank node.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `vcard:street-address` | `rdfs:Literal` typed as `xsd:string` | 0..1 | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | P E |
| `vcard:locality` | `rdfs:Literal` typed as `xsd:string` | 0..1 | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | P E |
| `vcard:region` | `rdfs:Literal` typed as `xsd:string` | 0..1 | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | P E |
| `vcard:country-name` | `rdfs:Literal` typed as `xsd:string` | 0..1 | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | P E |
| `vcard:postal-code` | `rdfs:Literal` typed as `xsd:string` | 0..1 |  | P E |

#### Person

`foaf:Person`

Use ORCID as URI.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:name` | `rdfs:Literal` typed as `xsd:string` | 1 | [Issue: consider breaking down into title, given name, family name?] | P E |
| `org:memberOf` | `foaf:Organization` ≡ `org:Organization` | 0..* |  | P E |
| `foaf:mbox` | `owl:Thing` | 0..1 | Use <mailto:> | P E |

#### Organization (FOAF) ≡ Organization (Org)

`foaf:Organization` ≡ `org:Organization`

Use ROR as URI.

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:name` | `rdfs:Literal` typed as `xsd:string` | 1..* | Must use a language tag (e.g. `@en`). May be duplicated in multiple languages. | P E |
| `foaf:mbox` | `owl:Thing` | 0..1 | Use <mailto:> | P E |

#### Location

`dcterms:Location`

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `locn:geometry` |  |  |  | N |
| `dcat:bbox` | `rdfs:Literal` typed as `geo:wktLiteral` | 0.. |  | E |
| `dcat:centroid` |  |  |  | N |

#### Period of Time

`dcterms:PeriodOfTime`

| Property | Range | Cardinality | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:startDate` | `rdfs:Literal` typed as `xsd:gYear`, `xsd:gYearMonth`, `xsd:date` or `xsd:dateTime` | 0..1 |  | E |
| `dcat:endDate` | `rdfs:Literal` typed as `xsd:gYear`, `xsd:gYearMonth`, `xsd:date` or `xsd:dateTime` | 0..1 |  | E |
| `time:hasBeginning` |  |  |  | N |
| `time:hasEnd` |  |  |  | N |

#### Activity

`prov:Activity`

#### Classes not used

Cos we don't instantiate them explicitly although they are the range of a predicate of a main class.

`foaf:Document`

`dcterms:LinguisticSystem`

`dcterms:LicenseDocument`

`dcterms:Standard`

`dcterms:MediaType`

`dcterms:MediaTypeOrExtent`

`spdx:Checksum`
