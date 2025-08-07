---
title: Custom Imagery
---
Adding extra imagery layers is simple. In the `imagery` section, specify imagery layers as a map of maps:

```yaml
imagery:
  IPR-orotofoto-last-tms:
    name: "Praha IPR latest orthophoto"
    url: "https://osm-{switch:a,b,c}.zby.cz/tiles_ipr_last.php/{zoom}/{x}/{y}.jpg"
    maxZoom: 20
    attribution: "IPR Praha; OSM CZ"
  turkuOrto:
    name: "City of Turku ortophoto"
    type: wms
    url: "https://opaskartta.turku.fi/TeklaOGCWeb/WMS.ashx?FORMAT=image/png&TRANSPARENT=TRUE&VERSION=1.1.1&SERVICE=WMS&REQUEST=GetMap&LAYERS=Ilmakuva 2021&STYLES=&SRS={proj}&WIDTH={width}&HEIGHT={height}&BBOX={bbox}"
    minZoom: 4
    maxZoom: 20
    attribution: "© Turun kaupunki"
  nommeTest:
    url: nomme-test.mbtiles
    maxZoom: 18
```

Available keys are:

* `url`: the only required key, layer URL templated the same as in the [editor layer index](https://github.com/osmlab/editor-layer-index/blob/gh-pages/CONTRIBUTING.md).
* `name`: how the layer is listed on the imagery panel.
* `type`: one of those:
    * `tms`: a regular tile service with URLs usually ending in `/{zoom}/{x}/{y}.png`
    * `wms`: a WMS service, like the `turkuOrto` in the example. This type is implied when the URL contains `SERVICE=WMS`.
    * `mbtiles`: a packaged [MBTiles](https://wiki.openstreetmap.org/wiki/MBTiles) file. This type is implied when the url (which should point to the file) ends with `.mbtiles`. Only raster tile packages are supported. A web URL would probably fail.
    * `vector`: a vector imagery layer, [see below](#vector-tiles).
* `icon`: an icon to show on the imagery panel.
* `attribution`: the string displayed on the screen when the layer is active.
* `minZoom` and `maxZoom`: self-explanatory, integer numbers.
* `wms4326`: a boolean field that should be true for WMS layers which support EPSG:4326, but not 3857.
* `tileSize`: for TMS layers, defaults to 256.
* `opacity`: layer opacity, between 0.0 and 1.0 (_since API 1.1_). Not all layer types support this. It is possible to have semi-transparent base tiles, and it would look weird.
* `headers`: HTTP headers to add when requesting tiles.
* `force`: when set to `true`, forces this imagery to be enabled when the plugin is installed, and on every app restart.

To change the base map layer, use `base` for an imagery key. For example:

```yaml
imagery:
  base:
    name: "Topo Map"
    url: ...
```

## Overlays

Initially Every Door displays a single map layer: either OpenStreetMap-based for the base map, or an imagery layer. For thematic or directed mapping you might need to add more layers: for example, a semi-transparent tile layer with a heatmap, or display polygons for an area to confine the mapping effort.

With plugins, it's pretty simple: add an `overlays` key with a list of layers in the same format.

```yaml
overlays:  
 - url: "https://tile.waymarkedtrails.org/cycling/{zoom}/{x}/{y}.png"  
   maxZoom: 17  
 - url: map.geojson
```

With overlays, you can also use a `geojson` type. It is automatically inferred when the URL ends with a `.geojson` or a `.json`. URLs _are_ supported, but if the `url` value does not start with `http`, it is assumed to be a file inside the plugin.

## Vector Tiles

_Since API 1.1_

You can reference vector tiles, and even package them, saving on space compared to raster tiles.
Those are added like regular imagery (anywhere: as a base layer, overlay, or even satellite),
and require at least two keys:

* `type: vector` is required.
* `url`: name of the style JSON file, either an URL or a file name inside the plugin.
* `name`, `icon`, `attribution`, `headers`: same as for raster layers.
* `key`: API key, should be referenced in style URLs as `{key}`.
* `fast`: if `true` (default), tiles are rendered into raster tiles, if `false` — they are
    re-rendered all the time, allowing for labels to be rotated, for example. As the name
    implies, non-fast mode is slow when there is a lot to draw.

Here is an example adding two layers: one external for the base layer, and one packaged
overlay:

```yaml
imagery:
  base:
    name: Positron
    url: https://tiles.openfreemap.org/styles/positron
    type: vector
    attribution: OpenFreeMap

overlays:
  - url: tallinn-overlay.json
    type: vector
```

A style can reference both external and packaged tiles (as MBTiles). Sprites can also
be packaged. Glyphs are not supported. Here is a sample style for the overlay mentioned above:

```json
{
  "id": "tallinn-overlay",
  "sources": {
    "mbtiles": {
      "type": "vector",
      "tiles": ["greater_tallinn.mbtiles"],
      "minzoom": 1,
      "maxzoom": 14
    }
  },
  "sprite": "sprite",
  "layers": [...]
}
```

It references `greater_tallinn.mbtiles`, `sprite.json`, and `sprite.png` packaged into the
root of the plugin archive.
