# Orbit search: geocode an address

Queries run against the Postman API Network via Orbit MCP `search`.

## Query 1 — "geocode an address to latitude and longitude coordinates"

### Geocode a set of addresses (Esri)
- **Method:** POST
- **URL:** https://geocode-api.arcgis.com/arcgis/rest/services/World/GeocodeServer/geocodeAddresses
- **ID:** `urn:orbit:endpoint:v1:1I4Uh1tYHMK5wIlJkgG3SsddzdPg120qLbT8jPv9KZ4Px1KW7Dr68cWfyziqn:esri:geocode-a-set-of-address`
- **Evaluate Guide:** Batch geocodes multiple addresses through ArcGIS World Geocoding and returns matched location data.
  - Use for: batch address geocoding, coordinate lookup, address matching
  - Not supported: reverse geocoding, routing, map rendering

### Geocode location A (Esri)
- **Method:** GET
- **URL:** https://geocode-api.arcgis.com/arcgis/rest/services/World/GeocodeServer/findAddressCandidates
- **ID:** `urn:orbit:endpoint:v1:MXWIl9HGTDmcWiItQRwD3gUh3ZEQSdm8FPNlbZA7YDStdvUN3KMgFMNQezk:postman:geocode-location-a`
- **Evaluate Guide:** Geocodes one address with ArcGIS findAddressCandidates and returns candidate locations ranked by match quality.
  - Use for: single-address geocoding, candidate matching, coordinate lookup
  - Not supported: batch geocoding, reverse geocoding, route planning

### Forward Geocode (Geocode Address → Coordinates) (Radar)
- **Method:** GET
- **URL:** https://api.radar.io/v1/geocode/forward
- **ID:** `urn:orbit:endpoint:v1:aGr31osDAu5ruWnQqtVGDCATuOKMjyvj:radar:forward-geocode-geocode`
- **Evaluate Guide:** No `evaluateGuide` returned. Description notes publishable-key auth, `query` as the only required
  param, optional `layers` / `country` / `lang`, confidence values of exact/interpolated/fallback, and a default
  rate limit of 10 rps.

### Geocode address (Rhombus Systems)
- **Method:** POST
- **URL:** https://api2.rhombussystems.com/api/location/geoCode
- **ID:** `urn:orbit:endpoint:v1:1JhNlBQCc1sts2sRHe0epV5cfTdtKjMEzGhhcubCitLILYU4Cm3nWPgPvB3Gm:rhombus:geocode-address`
- **Evaluate Guide:** Converts an address into latitude and longitude coordinates using the Rhombus location service.
  - Use for: address geocoding, coordinate lookup, location normalization
  - Not supported: reverse geocoding, routing, map display

### Lookup the latitude,longitude of an address (HPE Aruba Networking)
- **Method:** GET
- **URL:** https://172.16.3.20/gms/rest/location/addressToLocation
- **ID:** `urn:orbit:endpoint:v1:1MvBXWcfBS1o5lqsUf9Fzwd5ermbKHMyh53VK0F3WSQNfBEaTiSPH2vyX3RbM:hpe:lookup-the-latitude-long`
- **Evaluate Guide:** Resolves an address to an ordered array of matching latitude and longitude locations in EdgeConnect SD-WAN.
  - Use for: address geocoding, best-match lookup, location resolution
  - Not supported: reverse geocoding, routing, network configuration
  - Note: private RFC1918 host — appliance-local, not a public service.

Reverse-geocoding results also returned by this query (not a fit for forward geocoding): Pinball Map,
Cloudmersive, Vedika, OpenWeather, Meteoprog, OneMap SG, Esri reverseGeocode.

## Query 2 — "forward geocoding convert street address string into coordinates"

### geocode (Google)
- **Method:** GET
- **URL:** https://maps.googleapis.com/maps/api/geocode/json
- **ID:** `urn:orbit:endpoint:v1:1LKGXlgOcG8yPK3VhFCHNE60JlxB03MXIW5CoS2ccJgvfeT904cCPC7A9Jhwn:google:geocode`
- **Evaluate Guide:** Supports address-to-coordinate and coordinate-to-address geocoding, including lookups by place identifier.
  - Use for: forward geocoding, reverse geocoding, place ID lookup, mapping addresses
  - Not supported: navigation, distance matrices, map imagery

### Forward Geocoding - Search (Address to Coordinates) (APIFreaks)
- **Method:** GET
- **URL:** https://api.apifreaks.com/v1.0/geocoder/search
- **ID:** `urn:orbit:endpoint:v1:aNIlwdaRLputqrxxn10U3re76azr1Y0s:apifreaks:forward-geocoding-search`
- **Evaluate Guide:** Finds coordinates and structured location details from a free-form address, place, or landmark query.
  - Use for: geocoding addresses, locating landmarks, resolving place names, getting bounding boxes
  - Not supported: coordinate-to-address lookup, routing, map tiles

### Forward Geocoding (Address to Coordinates) (APIFreaks — duplicate variant)
- **Method:** GET
- **URL:** https://api.apifreaks.com/v1.0/geocoder/search
- **ID:** `urn:orbit:endpoint:v1:aNIlwdqeBGW8B9diAdC77BeVBZKt9iWC:apifreaks:forward-geocoding-addres`
- **Evaluate Guide:** Finds WGS84 coordinates and structured address details from free-form address, place, or business queries.
  - Use for: geocoding addresses, finding businesses, resolving place names, limiting results
  - Not supported: coordinate-to-address lookup, routing, map tiles

### Search address (Esri)
- **Method:** GET
- **URL:** https://geocode-api.arcgis.com/arcgis/rest/services/World/GeocodeServer/findAddressCandidates
- **ID:** `urn:orbit:endpoint:v1:1I4Uh1tYHMK5wC6IltepH9qOrLRZfOyXTl8wroVvRNLKfjqCWiuY530ygaI1g:esri:search-address`
- **Evaluate Guide:** Geocodes one address or place description and returns matching location candidates.
  - Use for: address search, place lookup, finding candidate coordinates
  - Not supported: batch geocoding, reverse geocoding, route planning

### Search street intersection (Esri)
- **Method:** GET
- **URL:** https://geocode-api.arcgis.com/arcgis/rest/services/World/GeocodeServer/findAddressCandidates
- **ID:** `urn:orbit:endpoint:v1:1I4Uh1tYHMK5x11h3g8SR0wVANRygVh6H2T5Wg3f1cisEC2FG2OHgtvn0eNxo:esri:search-street-intersecti`
- **Evaluate Guide:** No `evaluateGuide` returned. Same operation as Search address with `category=intersection`.

### Get Address (PositionStack, via a Postman demo collection)
- **Method:** GET
- **URL:** http://api.positionstack.com/v1/forward
- **ID:** `urn:orbit:endpoint:v1:1I4UV9Fqy4Y3DrgUgFkLIEJ18gBzKrO9HqAFQSw8yzykvkIvK4d36OIcgnwMY:more:get-address`
- **Evaluate Guide:** No `evaluateGuide` returned. Plain HTTP and wrapped in a Slack-bot demo collection.

## Coverage notes

- Both queries reported `meta.total: 15` with a `nextCursor` present; pagination caps at 40 results per query.
  Further pages were not fetched.
- Neither query surfaced Mapbox, HERE, OpenCage, or Nominatim.
