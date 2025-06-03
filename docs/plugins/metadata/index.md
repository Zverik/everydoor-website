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

## Imagery

List layers in `imagery` and `overlays` sections. See [imagery](imagery.md).

## Element Kinds

An "element kind" is a set of rules that apply to tags to split objects into groups. Before the refactoring, some groups were made to non-intersect, but that can be untrue now. Each element kind has a string name, and a set of tag matching rules.

New and overridden element kinds go under a `kinds` section, which is documented in [element\_kinds](element_kinds.md).

## Modes

Modes are listed in the `modes` entry as maps: keys are mode identifiers, and values are maps of key-value mode settings.

See [modes](modes.md) for a detailed explanation.

## Presets and Fields

Those go into `presets` and `fields` sections. All explained in [presets](presets.md).

