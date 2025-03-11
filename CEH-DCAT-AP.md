# CEH DCAT Application Profile

Last updated: 11/03/2025
Contributors: Helen Rawsthorne

## Introduction

The EIDC holds records for datasets. Each record can be accessed in multiple formats including Turtle, which is used to express RDF data. The aim of this application profile is to provide a model that can be used to describe all resources at CEH, so as to standardise them across the organisation and align them with community best practices. The model is an application profile of the DCAT 3 vocabulary. It overlaps a lot with DCAT-AP 3.0, but we can not use that as it is specific to data portals in the EU so is too restrictive in parts.

## Application Profile Specification

Reuse column
- A: reused as defined and expressed in DCAT 3
- E: reused with additional usage notes or additional restrictions compared to DCAT 3
- N: in DCAT 3 but not in CEH-DCAT-AP
- P: CEH-DCAT-AP profile specific extension

Prefixes

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
| rdfs | http://www.w3.org/2000/01/rdf-schema# |
| time | http://www.w3.org/2006/time# |
| vcard | http://www.w3.org/2006/vcard/ |
| xsd | http://www.w3.org/2001/XMLSchema# |

### Main Classes

#### Catalog

`dcat:Catalog`

This class is a sub-class of `dcat:Dataset` and of `dcat:Resource`.

Every instance of this class must also be an instance of `prov:Entity`.

| Property | Range | Card. | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `foaf:homepage` | `foaf:Document` | 1 |  | A |
| `dcat:themeTaxonomy` | `rdfs:Resource` |  | TODO | E |
| `dcat:resource` | `dcat:Resource` | 0..* |  | A |
| `dcat:dataset` | `dcat:Dataset` | 0..* |  | A |
| `dcat:service` | `dcat:DataService` | 0..* |  | A |
| `dcat:catalog` | `dcat:Catalog` | 0..* |  | A |
| `dcat:record` | `	dcat:CatalogRecord` | 0..* |  | A |

#### Cataloged Resource

`dcat:Resource`

This class is a super-class of `dcat:Dataset`, of `dcat:DataService` and of `dcat:Catalog`.

Every instance of the class `dcat:Resource`, or one of its sub-classes, must also be an instance of `prov:Entity`. This is because some properties present in this application profile have `prov:Entity` as their domain (e.g. `prov:qualifiedAttribution`, `prov:wasGeneratedBy`).

| Property | Range | Card. | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcterms:accessRights` | `dcterms:RightsStatement` | 1 |  | A |
| `dcterms:conformsTo` |  | .. | TODO |  |
| `dcat:contactPoint` | `vcard:Kind` | 1..* | Mandatory. [Issue: Will probably always be a `vcard:Organization` if we don't want to give emails of individuals?] | E |
| `dcterms:creator` | `foaf:Agent` | 0..* | Optional. One of creator and contributor mandatory. If used, must be a `foaf:Agent`. Wherever possible, use ORCID for individuals and ROR for organisations. If ROR doesn't exist, submit a request to add the organisation. | E |
| `dcterms:description` | `rdfs:Literal` typed as `xsd:string` | 1..* | Mandatory in at least one language. Must use a language tag (e.g. `@en`). | E |
| `dcterms:title` | `rdfs:Literal` typed as `xsd:string` | 1..* | Mandatory in at least one language. Must use a language tag (e.g. `@en`). | E |
| `dcterms:issued` | `rdfs:Literal` typed as `xsd:date` | 1 | Mandatory. If embargoed, use date when metadata record put online, not date of end of embargo. | E |
| `dcterms:modified` | `rdfs:Literal` typed as `xsd:date` | .. | [Issue: DCAT says this indicates a change to the resource, not just the catalog record... shouldn't this be a new version? think about this] | E |
| `dcterms:language` | From https://id.loc.gov/vocabulary/iso639-1.html | 1..* | Mandatory. If representations of a dataset are available for each language separately, define an instance of dcat:Distribution for each language and describe the specific language of each distribution using dcterms:language (i.e., the dataset will have multiple dcterms:language values and each distribution will have just one as the value of its dcterms:language property). In case of multilingual distributions, the distributions will have multiple dcterms:language values.  | E |
| `dcterms:publisher` | `foaf:Agent` | 1..* | Wherever possible, use ORCID for individuals and ROR for organisations. If ROR doesn't exist, submit a request to add the organisation. [Issue: Will probably always be a `foaf:Organization`?] | E |
| `dcterms:identifier` | `rdfs:Literal` typed as `xsd:anyURL` | 1..* | Mandatory, at least the catlogue-specific PID given to the resource. If the resource has a DOI, it must be stated here too. | E |
| `dcat:theme` |  | .. | TODO |  |
| `dcterms:type` |  | .. | [Issue: currently we use terms from https://www.dublincore.org/specifications/dublin-core/dcmi-terms/#section-7, but maybe we should migrate to https://schema.datacite.org/meta/kernel-4.1/include/datacite-resourceType-v4.1.xsd as it includes `Workflow` and `Model`, neither of which are in dcterms? If we do that, do not include term `Other`. Both vocabs are recommended by DCAT for this predicate.] |  |
| `dcterms:relation` |  |  |  | N |
| `dcat:qualifiedRelation` |  |  |  | N |
| `dcat:keyword` |  | .. | TODO |  |
| `dcat:landingPage` | `foaf:Document` | 1..* | Mandatory. Must only link to the catalogue in which the resource was deposited originally. [Issue: does it make sense to allow putting two links that direct to the same page? E.g. DOI and catalog PID.] | E |
| `prov:qualifiedAttribution` |  |  |  | N |
| `dcterms:license` | `dcterms:LicenseDocument` | 1 |  | A |
| `dcterms:rights` | `dcterms:RightsStatement` | 0..1 |  | A |
| `dcterms:hasPart` |  |  |  | N |
| `odrl:hasPolicy` |  |  |  | N |
| `dcterms:isReferencedBy` |  | 0..* | One per publication (do not put DOI and another PID of the same resource). Use DOI where possible, second best option is other PID. | E |
| `dcat:previousVersion` |  | 0..1 | PID of the previous version of the resource. Equivalent to `pav:previousVersion` but must only define `dcat:previousVersion`. May be used without using `dcterms:replaces`. | E |
| `dcat:hasVersion` |  |  |  | N |
| `dcat:hasCurrentVersion` |  |  |  | N |
| `dcterms:replaces` |  | 0..1 | PID of the resource being replaced. If used, `dcat:previousVersion` must also be used. | E |
| `dcat:version` |  | 1 | Mandatory. String, semantic versionning? SemVer, SchemaVer, https://www.ands.org.au/working-with-data/data-management/data-versioning |  |
| `adms:versionNotes` | `rdfs:Literal` typed as `xsd:string` | 0..1 | Mandatory if previous version exists. | E |
| `adms:status` |  | .. | [Issue: choose a vocabulary to use out of: Those defined in the ISO standard for item registration [ISO-19135] (accepted / not accepted, deprecated, experimental, reserved, retired, stable, submitted, superseded, valid / invalid). The progress codes defined in [ISO-19115] (accepted, completed, deprecated, final, historical archive, not accepted, obsolete, ongoing, pending, planned, proposed, required, retired, superseded, tentative, under development, valid, withdrawn). The ADMS Status vocabulary [ADMS-SKOS], used in [DCAT-AP], which includes four statuses: completed, deprecated, under development, and withdrawn. The dataset statuses [EUV-DS] and concept statuses [EUV-CS] vocabularies from the EU Vocabularies registry.] | E |
| `dcat:first` |  |  |  | N |
| `dcat:last` |  |  |  | N |
| `dcat:prev` |  |  |  | N |

[Issue: where to find Custodian and Owner properties (present in html) -> vCard (organisation)?]

[Issue: how to indicate last time metadata record updated?]

#### Catalog Record

`dcat:CatalogRecord`

Not used.

#### Dataset

`dcat:Dataset`

This class is a sub-class of `dcat:Resource` and a super-class of `dcat:Catalog`.

Every instance of this class must also be an instance of `prov:Entity`.

| Property | Range | Card. | Usage | Reuse |
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

| Property | Range | Card. | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcterms:title` | `rdfs:Literal` typed as `xsd:string` | 1..* | Mandatory in at least one language. Must use a language tag (e.g. `@en`). | E |
| `dcterms:description` | `rdfs:Literal` typed as `xsd:string` | 1..* | Mandatory in at least one language. Must use a language tag (e.g. `@en`). | E |
| `dcterms:issued` | `rdfs:Literal` typed as `xsd:date` | 1 | Mandatory. If embargoed, use date when metadata record put online, not date of end of embargo. | E |
| `dcterms:modified` | `rdfs:Literal` typed as `xsd:date` | .. | [Issue: DCAT says this indicates a change to the resource, not just the catalog record... shouldn't this be a new version? think about this] | E |
| `dcterms:license` | `dcterms:LicenseDocument` | 1 | Mandatory. | E |
| `dcterms:accessRights` | `dcterms:RightsStatement` | 1 | Mandatory. | E |
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

| Property | Range | Card. | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:endpointURL` | `rdfs:Resource` | 1 |  | A |
| `dcat:endpointDescription` | `rdfs:Resource` | 1..* |  | A |
| `dcat:servesDataset` | `dcat:Dataset` | 0..* |  | A |

### Secondary Classes

#### Location

`dcterms:Location`

| Property | Range | Card. | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `locn:geometry` |  |  |  | N |
| `dcat:bbox` | `rdfs:Literal` typed as `geo:wktLiteral` | 0.. |  | E |
| `dcat:centroid` |  |  |  | N |

#### Period of Time

`dcterms:PeriodOfTime`

| Property | Range | Card. | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `dcat:startDate` | `rdfs:Literal` typed as `xsd:date` | 0..1 |  | E |
| `dcat:endDate` | `rdfs:Literal` typed as `xsd:date` | 0..1 |  | E |
| `time:hasBeginning` |  |  |  | N |
| `time:hasEnd` |  |  |  | N |

#### License Document

`dcterms:LicenseDocument`

Not used.

#### Rights Statement

`dcterms:RightsStatement`

| Property | Range | Card. | Usage | Reuse |
| --- | --- | --- | --- | --- |
| `odrs:attributionURL` |  | 0..1 | Use when predicate is `dcterms:accessRights`. Choose a term from following vocab: https://inspire.ec.europa.eu/metadata-codelist/LimitationsOnPublicAccess. | P E |
| `odrs:copyrightHolder` |  |  |  | P A |
| `odrs:copyrightStatement` |  |  |  | P A |
| `odrs:attributionText` | `rdfs:Literal` typed as `xsd:string` | 0..1 | Use when predicate is `dcterms:accessRights`. Use text from `label` in vocab. | P E |
| `odrs:copyrightNotice` |  | 0..1 | Use when predicate is `dcterms:rights`. | P E |
| `odrs:copyrightYear` |  |  |  | P A |
