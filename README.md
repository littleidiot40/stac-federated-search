# Federated Search Extension Specification

- **Title:** Federated Search
- **Identifier:** <https://stac-extensions.github.io/template/v1.0.0/schema.json>
- **Field Name Prefix:** col
- **Scope:** Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @littleidiot40 @ycespb

This document explains the Federated Search Extension to the [SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) 
(STAC) specification.
This extension provides support fo the federated discovery use case. 
Leverage: [OGC records](http://docs.ogc.org/DRAFTS/20-004.html#_query_parameters)

- Examples:
  - [Item example](examples/item.json): Shows the basic usage of the extension in a STAC Item
  - [Collection example](examples/collection.json): Shows the basic usage of the extension in a STAC Collection
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)



## Landing Page

The following Link relation must exist in the Landing Page (root).

| **rel**  | **href**  | **type** | **From**               | **Description**             |
| -------- | --------- | ---------- | ------------ | --------------------------- |
| `search` | `/collections` |  `application/json`  | Extension | **REQUIRED** URI for the (STAC) Collection Search endpoint |

This `search` link relation must have a `type` of `application/json`. It is assumed to represent a GET request.  The collection `search` link can be distinguished from a regular item `search` link as the `type` for the item search should be `application/geo+json` instead.

## API Collection Search

### Endpoints

This extension also requires the endpoint below to be implemented as a [`local resources catalogue`](http://docs.ogc.org/DRAFTS/20-004.html#_tldr_local_resources_catalogue).

| Endpoint  | Returns         | Description     |
| --------- | --------------- | --------------- |
| `/collections` | Collection Collection | Collection Search endpoint.  When invoked without any query parameters, no filter is applied. |

The response format is `application/json` and is an extension of the the /collections reponse defined bu [OGC API-Features](https://docs.opengeospatial.org/is/17-069r3/17-069r3.html).  See [OGC API-Records §6.3](http://docs.ogc.org/DRAFTS/20-004.html#_tldr_local_resources_catalogue) where the endpoint `/collections` is provided as typical example of a `local resources catalogue`.  See also  §9 "Simple Query" of [OGC API - Common - Part 2: Geospatial Data](https://docs.ogc.org/DRAFTS/20-024.html#rc-simple-query-section) for additional information about the expected response content. 
 
### Query Parameters and Fields

The following list of parameters is used to narrow search queries. They can all be represented as query 
string parameters in a GET request (**REQUIRED**), or as JSON entity fields in a POST request. 

The core parameters for STAC collection search are borrowed from the [STAC Item Search](https://github.com/radiantearth/stac-api-spec/item-search).  This extension adds a few additional parameters for convenience.

| Parameter   | Type             | Source API | Description                                                                                                                                                                     |
| ----------- | ---------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| limit       | integer          | OAFeat     | **REQUIRED** The maximum number of results to return (page size).                                                                                                                            |
| bbox        | \[number]        | OAFeat     | **REQUIRED** Requested bounding box.                                                                                                                                                         |
| datetime    | string           | OAFeat     | **REQUIRED** Single date+time, or a range ('/' separator), formatted to [RFC 3339, section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). Use double dots `..` for open date ranges. |
| intersects  | GeoJSON Geometry | STAC       | Searches Collections by performing intersection between their geometry and provided GeoJSON geometry.  All GeoJSON geometry types must be supported.                                  |
| ids         | \[string]        | STAC       | **REQUIRED** Array of Collection ids to return.                                                                                                                                                    |
| q | \[string]         | OGC API-Records       | **REQUIRED** String value for textual search.        
| type | \[string]        | OGC API-Records       | Resource type.      
| externalId | \[string]         | OGC API-Records       | External identifier associated with the collection. (same as `ids` ?)                                                                                                |



## STAC Collections

### Collection Properties

See [STAC Collection Specification](https://github.com/radiantearth/stac-spec/blob/master/collection-spec/collection-spec.md).

Note that [OGC API-Records §6.7](http://docs.ogc.org/DRAFTS/20-004.html#sc_record-collection-overview) defines a different list of possible fields for Collection in §6.7, e.g. `publisher` instead of `providers`.  We will have to propose the actual list of mandatory elmements if mixing STAC and API-Records. 


### Collection Links

The following Link relations must exist in the Collection as [Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| **rel**  | **href**  | **type** | **From**               | **Description**             |
| -------- | --------- | --------- | ------------- | --------------------------- |
| `items` | `/collections/{collection-id}/items` | `application/geo+json` | OAFeat | **REQUIRED** URI for the Item Search endpoint as per [§8.1.3 of OGC API-Records](http://docs.ogc.org/DRAFTS/20-004.html#_links). |

TBD: do we prefer `items` or `search` ?   `search` is defined by SATC but only allowed in the root (Landing page).  There are more parameters defined than in OAFeat.

This `search` link relation must have a `type` of `application/geo+json`. It is assumed to represent a GET request.  The collection `search` link can be distinguished from a regular item `search` link as the `type` for the item search should be `application/geo+json` instead.

### Collection Assets

| **role**  | **type**                           | **Description**             |
| -------- | ----------------------------------- | --------------------------- |
| `metadata` | `application/dif10+xml` |  Collection metadata file in DIF-10 format. |



## API Item Search

The API for Item Search is derived from [STAC Item Search](https://github.com/radiantearth/stac-api-spec/item-search) and [OGC API-Features](https://docs.opengeospatial.org/is/17-069r3/17-069r3.html).
The current section defines the mandatory requirements.


### Endpoints

This conformance class also requires the endpoint below to be implemented.

| Endpoint  | Returns         | Description     |
| --------- | --------------- | --------------- |
| `/collections/{collection-id}/items` | Item Collection | Item Search endpoint.  |
 
### Query Parameters and Fields

The core parameters for STAC collection search are borrowed from the [STAC Item Search](https://github.com/radiantearth/stac-api-spec/item-search).  This extension adds a few additional parameters for convenience.

| Parameter   | Type             | Source API | Description                                                                                                                                                                     |
| ----------- | ---------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| limit       | integer          | OAFeat     | **REQUIRED** The maximum number of results to return (page size).                                                                                                                            |
| bbox        | \[number]        | OAFeat     | **REQUIRED** Requested bounding box.                                                                                                                                                         |
| datetime    | string           | OAFeat     | **REQUIRED** Single date+time, or a range ('/' separator), formatted to [RFC 3339, section 5.6](https://tools.ietf.org/html/rfc3339#section-5.6). Use double dots `..` for open date ranges. |
| intersects  | GeoJSON Geometry | STAC       | Searches Collections by performing intersection between their geometry and provided GeoJSON geometry.  All GeoJSON geometry types must be supported.   TBD: this allowed on a STAC /search endpoint and not on an OAFeat /items endpoint ?   |
| ids         | \[string]        | STAC       | **REQUIRED** Array of item ids to return.     TBD: this allowed on a STAC /search endpoint and not on an OAFeat /items endpoint ?     | 
| externalId | \[string]         | OGC API-Records       | External identifier associated with the item. (same as `ids` ?)  



## STAC Items


### Item Properties 

Which item properties from which STAC extensions will CEOS recommend ?  A list of possibilities is shown on https://github.com/stac-utils/stac-crosswalks/tree/master/OGC_17-003r2.

[CEOS Best Practices](https://ceos.org/document_management/Working_Groups/WGISS/Documents/WGISS%20Best%20Practices/CEOS%20OpenSearch%20Best%20Practice.pdf) CEOS-BP-12, CEOS-BP-12B, CEOS-BP-12C, CEOS-BP-12D and CEOS-BP-12E can be implemented using Link objects or Asset objects.  The following recommendations apply. 


### Item Links

TBD.

### Item Assets

`CEOS-BP-012C`

| **role**  | **type**                           | **Description**             |
| -------- | ----------------------------------- | --------------------------- |
| `metadata` | `application/vnd.iso.19139+xml` |  Granule metadata file in ISO 19139 format. |
| `metadata` | `application/gml+xml;profile=http://www.opengis.net/spec/EOMPOM/1.1` |  Granule metadata file in OGC 10-157r4 format. |

`CEOS-BP-012D`

| **role**  | **type**                           | **Description**             |
| -------- | ----------------------------------- | --------------------------- |
| `metadata` | various |  Granule metadata file in particular format indicated by media type. |
| `thumbnail` | various  |  Thumbnail image. |
| `overview` | various  |  Quicklook or browse image.  Image of the data typically used for making data request decisions |
| `data` | various  |  Data file or other science data resource; may be large in size. |


## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```
