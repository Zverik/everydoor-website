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
* `name`: how the layer is listed in the imagery panel.
* `type`: one of those:
    * `tms`: a regular tile service with URLs usually ending in `/{zoom}/{x}/{y}.png`
    * `wms`: a WMS service, like the `turkuOrto` in the example. This type is implied when the URL contains `SERVICE=WMS`.
    * `mbtiles`: a packaged [MBTiles](https://wiki.openstreetmap.org/wiki/MBTiles) file. This type is implied when the url (which should point to the file) ends with `.mbtiles`. Only raster tile packages are supported. A web URL would probably fail.
* `attribution`: the string displayed on the screen when the layer is active.
* `minZoom` and `maxZoom`: self-explanatory, integer numbers.
* `wms4326`: a boolean field that should be true for WMS layers which support EPSG:4326, but not 3857.
* `tileSize`: for TMS layers, defaults to 256.
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
