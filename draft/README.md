# GeocodeJSON-spec

**Revision**	0.1

**Copyright**	This work is licensed under a [Creative Commons Attribution License (CC0)](https://creativecommons.org/about/cc0)

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in
this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

## 1. Purpose

This specification attempts to create a standard for handling geocoding results.

## 2. File format

`GeocodeJSON` is a an extension of the [GeoJSON](http://geojson.org/) standard
that aim to define places results. As such, files implementing `GeocodeJSON`
MUST be valid GeoJSON and valid [JSON](http://json.org/) files. The JSON
keys described here are not exclusive.


### Main object

```javascript
{

  // REQUIRED. GeocodeJSON result is a FeatureCollection.
  "type": "FeatureCollection",

  // REQUIRED. Namespace.
  "geocoding": {

    // REQUIRED. A semver.org compliant version number. Describes the version of
    // the GeocodeJSON spec that is implemented by this instance.
    "version": "0.1.0",

    // OPTIONAL. Default: null. The licence of the data. In case of multiple sources,
    // and then multiple licences, can be an object with one key by source.
    // Can be a freeform text property describing the licensing details.
    // Can be a URI on the server, which outlines licensing details.
    "licence": "http://pelias.mapzen.com/license",

    // OPTIONAL. Default: null. The attribution of the data. In case of multiple sources,
    // and then multiple attributions, can be an object with one key by source.
    // Can be a URI on the server, which outlines attribution details.
    "attribution": "http://pelias.mapzen.com/attribution",

    // OPTIONAL. Default: null. The query that has been issued to trigger the
    // search.
    // Freeform object.
    // This is the equivalent of how the engine interpreted the incoming request.
    // Helpful for debugging and understanding how the input impacts results.
    "query": "24 allée de Bercy 75012 Paris",
    // OR
    "query": {
      "text": "24 allée de Bercy 75012 Paris",
      "address": {
        "number": 24,
        "street": "allée de Bercy",
      },
      "size": 10,
      "layers": "admin1"
    },
    
    // OPTIONAL
    // Freeform
    "engine": {
      "name": "Pelias",
      "version": 0.2.0
    }
  },

  // REQUIRED. As per GeoJSON spec.
  "features": [
    // OPTIONAL. An array of feature objects. See below.
  ]
}
```

### Feature object

```javascript
{

  // REQUIRED. As per GeoJSON spec.
  "properties": {

    // REQUIRED. Namespace.
    "geocoding": {

      // REQUIRED. One of "house", "street", "locality", "city", "region", "country".
      // TODO: make a clean list of common cases, plus make clear that the list
      // isn't meant to be closed.
      "type": "house",

      // OPTIONAL. Result accuracy, in meters.
      "accuracy": 20,

      // RECOMMENDED. Suggested label for the result.
      "label": "My Shoes Shop, 64 rue de Metz 59280 Armentières",

      // OPTIONAL. Name of the place.
      "name": "My Shoes Shop",

      // OPTIONAL. Housenumber of the place.
      // TODO: what about the suffix (64A, 64 bis, etc.)?
      "housenumber": "64",

      // OPTIONAL. Street of the place.
      "street": "Rue de Metz",

      // OPTIONAL. Postcode of the place.
      "postcode": "59280",

      // OPTIONAL. City of the place.
      "city": "Armentières",

      // OPTIONAL. District of the place.
      "district": null,

      // OPTIONAL. County of the place.
      "county": null,

      // OPTIONAL. State of the place.
      "state": null,

      // OPTIONAL. Country of the place.
      "country": "France",

      // OPTIONAL. Administratives boundaries the feature is included in,
      // as defined in http://wiki.osm.org/wiki/Key:admin_level#admin_level
      // TODO is there some still generic but less OSMish scheme?
      "admin": {
        "level2": "France",
        "level4": "Nord-Pas-de-Calais",
        "level6": "Nord"
      },

      // OPTIONAL. Geohash encoding of coordinates (see http://geohash.org/site/tips.html).
      "geohash" : "Ehugh5oofiToh9aWe3heemu7ighee8",
    }

  },

  // REQUIRED. As per GeoJSON spec.
  "type": "Feature",

  // REQUIRED. As per GeoJSON spec.
  "geometry": {
    "coordinates": [
      2.889957,
      50.687328
    ],
    "type": "Point"
  }
}
```
