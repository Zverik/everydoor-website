# Metadata Keys

The `plugin.yaml` file is a simple key-value dictionary. There are some top-level keys, like a plugin name, and sections relating to various configurable subsystems.

Top level keys are:

* `id`: the plugin identifier. Should be different from other identifiers (otherwise this plugin would replace another with the same id), and consist of characters, numbers, and maybe some punctuation. This is the only required key.
* `name`: plugin name. Can be translated (see below).
* `version`: numeric version, floating point.
* `description`: long-form description of what this plugin does.
* `author`: who wrote the plugin.
* `icon`: an icon for the plugin (see below).
* `homepage`: a link to GitHub or another website to read about the plugin.
* `source`: a link to download the recent version of the plugin.

## Icons

Some sections expect an `icon` key. The value should be a file name of an image packaged with the plugin, in the `icons` subdirectory. For example, if you specify `icon: cycling.svg`, then the editor would look for `icons/cycling.svg` file in the plugin archive.

Formats supported are SVG (and its binary SI representation), PNG, GIF, and WebP.

Note that to make an SVG file re-colorable, you would need to add an attribute to either its elements, or to the `<svg>` outer tag: `stroke="currentColor"` and/or `fill="currentColor"`. This "current color" is what will be replaced with the icon color value.

Raster images are converted to black-and-white on its transparency layer, and the color is set to all non-transparent pixels.

_Binary SI images (compiled SVG) are possible to create with [jovial_svg](https://pub.dev/packages/jovial_svg_transformer), but better wait until there is a packaging infrastructure for plugins._

## Translations

You can provide labels in many languages for a plugin. To add a translation
into a language, create a top-level directory in the plugin package named `langs`,
and inside it, files named with language codes, e.g. `es.yaml`, `pt-BR.yaml`.

Each file should have the same structure as the `plugin.yaml`, but keeping
just keys that need translation. Those usually are `name`, `label`, `labels`,
`placeholder` etc. Some keys contain lists of strings instead of single
string values, e.g. `labels` for preset fields. Translate them as usual,
keeping the list in order.

## Imagery

Read the [imagery](imagery.md) section when you want to:

* Add a new base or overlay raster tile or WMS layer.
* Include a GeoJSON file and display it in the app.
* Package an MBTiles raster tile database with the plugin.

## Element Kinds

Read the [element kinds](element_kinds.md) section to:

* Add support for non-amenity tags, e.g. indoor or parking.
* Understand why some objects are not displayed.
* Define tag groups for some editing modes.

## Modes

Read the [modes](modes.md) section to:

* Customize colors and markers in the micromapping mode.
* Update the default preset lists.
* Create a custom entrances-like mode for dropping objects on the map.

## Presets and Fields

Read the [presets](presets.md) section to:

* Add a custom preset that shouldn't go into the global presets repository.
* Add a custom field and employ it in some presets.
* Define fields for a new entrances-like mode.
