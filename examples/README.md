# Notes on examples

The examples portray a notional case where a user performs a search for collections at a federating STAC API (CMR/IDN for example) as follows,

`GET https://federating.stac.api.com/stac/collections?q=ISRO&bbox=foo`

The results of that search are rendered in [collection.json](collection.json)
If the user then used the search rel=items link to perform a second search as follows,

`GET https://uops.nrsc.gov.in/stac/collections/C1_PANA_STUC00GTD.v1/items?bbox=foo`

The results of that search are rendered in item.json.

The available search parameters for collection search can be retrieved as follows.

`GET https://federating.stac.api.com/stac/queryables`

[queryables.json](queryables.json) is an example response advertizing STAC search parameters and additional parameters supported 
by this endpoint, e.g. `platform`, `instrument` and `organisationName` with the same meaning as in the 
corresponding [OpenSearch specification](https://docs.opengeospatial.org/is/13-026r9/13-026r9.html) and [CEOS Best Practice](https://ceos.org/document_management/Working_Groups/WGISS/Documents/WGISS%20Best%20Practices/CEOS%20OpenSearch%20Best%20Practice.pdf).

The available search parameters for granule search within a collection can be retrieved like this:

`GET https://uops.nrsc.gov.in/stac/collections/C1_PANA_STUC00GTD.v1/queryables`

[granule-queryables.json](granule-queryables.json) is an example response advertizing STAC search parameters and additional parameters supported 
by this endpoint.
