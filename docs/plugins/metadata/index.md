# Plugin Metadata

A plugin is a zip-archive with an `edp` extension, which includes a `plugin.yaml` file
inside at the top level. This section explains how to write it.

The `plugin.yaml` file is a simple key-value dictionary in YAML format.
There are some top-level keys, like a plugin name, and sections relating to
various configurable subsystems.

Top level keys are:

* `id`: the plugin identifier. Should be different from other identifiers (otherwise this plugin would replace another with the same id), and consist of latin characters, numbers, dashes, and underscores. This is the only required key.
* `name`: plugin name. Can be translated (see below).
* `version`: numeric version (see below).
* `description`: long-form description of what this plugin does. Split paragraphs with double newlines.
* `author`: who wrote the plugin.
* `icon`: an icon for the plugin (see below).
* `experimental`: `true` if the plugin should be hidden from the repository lists, `false` otherwise.
* `homepage`: a link to GitHub or another website to read about the plugin.
* `source`: a link to download the recent version of the plugin.
* `intro`: a text that will be shown after installing the plugin (not when installing from a file), and also available from the plugin page. Can contain any markdown, including images and links.

While only the `id` key is required, it's best to provide as many values as possible, at least for the first four: including `name`, `version`, and `description`. Here's for an example:

```yaml
id: cycling_infra
name: Cycling Infrastructure
version: 2
author: Ilya Zverev
icon: cycling.svg
description: |
  Testing plugin that adds a new mode to the editor, related to bicycle infrastructure.

  It resembles the building mode, but the primary (right) button adds a cycling barrier,
  and the secondary adds a bicycle parking stand. For the latter, capacity is shown
  on the map.
experimental: false
```

## Versioning Plugins

Plugin versions internally are all positive numbers, so it is perfectly valid to
use a through enumeration: `1`, `2`, and so on.

On the other hand, people are used to splitting major and minor releases, to
differentiate between big improvements and bug fixes. With that, you can use
that scheme for versioning, in the form of `<major>.<minor>`. For example,
`0.1`, `1.0`, `1.1`, and so on.

Internally those versions are converted into a single number by multiplying the
major version by a thousand, and adding a thousand. So `0.1` becomes `1001`,
and `2.0` — `3000`. This also means that after version `999` your plugin
might suddenly gain version `0.0`: that would probably be a good time
to switch to `1000.0`.

## Icons

Some sections expect an `icon` key. The value should be a file name of an image packaged with the plugin, in the `icons` subdirectory. For example, if you specify `icon: cycling.svg`, then the editor would look for `icons/cycling.svg` file in the plugin archive.

Formats supported are SVG, PNG, GIF, and WebP.
Good sources for SVG icons are [Material Icons](https://fonts.google.com/icons?icon.size=24&icon.color=%231f1f1f),
and [Maki](https://github.com/mapbox/maki/tree/main).
See [this list](https://github.com/ideditor/schema-builder/blob/main/ICONS.md) for
additional sources.

Note that to make an SVG file re-colorable, you would need to add an attribute to either its elements, or to the `<svg>` outer tag: `stroke="currentColor"` and/or `fill="currentColor"`. This "current color" is what will be replaced with the icon color value.

Raster images are converted to black-and-white on its transparency layer, and the color is set to all non-transparent pixels.

Another option for fast prototyping is to use an emoji character for an icon. Two drawbacks of that are, emoji
are not recolorable, and the alignment is slighly off. To use a character, find one
in [Emojipedia](https://emojipedia.org/), switch to the "Techinical information" tab,
and copy the codepoint sequence in form of `U+1F525` to the icon value.

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
