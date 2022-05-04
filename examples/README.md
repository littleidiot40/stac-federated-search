# Notes on examples

The examples portray a notional case where a user performs a search for collections at a federating STAC API (CMR/IDN for example) as follows,

`GET https://federating.stac.api.com/stac/collections?q=ISRO&bbox=foo`

The results of that search are rendered in [collection.json](collection.json)
If the user then used the search rel=items link to perform a second search as follows,

`GET https://uops.nrsc.gov.in/stac/collections/C1_PANA_STUC00GTD.v1/items?bbox=foo`

The results of that search are rendered in [item.json](item.json)
