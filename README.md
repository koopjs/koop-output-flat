# koop-output-flat
This output-services plugin converts responds with a "flat" json array. All nested GeoJSON properties are moved to the root level and given keys that are a path to their original nested location. For example, the following GeoJSON:

```
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": {
        "keywords": ["place", "residence"],
        "address": {
          "street": "187 Suffolk Ln.",
          "City": "Boise"
          }
      },
      "geometry": {
        "type": "Point",
        "coordinates": [
          -121.14486694336,
          47.884118004966
        ]
      }
    }
  ]
}
```

will get converted to:

```
[
    {
        "keywords.0": "place",
        "keywords.1": "residence",
        "address.street": "187 Suffolk Ln.",
        "address.City": "Boise"
    }
]
```

Note that geometry is omitted from the response
.
## Usage

Register this plugin with koop before your provider plugins to ensure that the Odata routes are bound to the providers.

```
const flat = require('@koopjs/output-flat')
koop.register(flat)

// Register providers
```

After startup, Koop will include an OData route for each registered provider.  For example, if you had registered the Github provider, the following route would be available:

`/github/:id/flat`
