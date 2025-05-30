---
title: Overriding Modes
---
Modes are listed in the `modes` entry as maps: keys are mode identifiers, and values are maps of key-value mode settings.

There are four mode types which are also the names of the four pre-defined modes: `micro`, `amenity`, `entrances`, `notes`. When your mode identifier is one of those, it doesn't add a new mode, but alters parameters of an existing mode.

For a new mode, you should specify `icon` and `iconOutlined` with a mode icon. The second is optional.

Note that not all parameters that you specify for a new mode can be adjusted for an existing mode.

### Micromapping

Things you can adjust:

* `kinds` (or `kind`, a string or a list of strings): objects that are edited in this mode. `micro` by default.
* `otherKinds` (or `otherKind`): objects that are shown as black dots on the map. `amenity` by default.
* `defaultPresets` (a list of strings): preset names for the list appearing after you tap the (+) button.
* `markers`: a map of preset name → marker definition, for overriding colors and icons in the legend and on the map. The definition can be either a `icon: icon_name` (see above), or a `color: color_name`.
    * We use [this library](https://pub.dev/packages/material_color_names) to resolve color names. Also several `ed`-prefixed colors are available, for those pre-built into the micromapping mode. For example, `edRed` and `edTeal`.
* `iconsInLegend`: when assigning an icon to a preset, it might be obvious from its appearance on the map. So when you do it at scale, the legend might become redundant. Setting this flag to `false` removes iconized items from the legend.

### Amenity

This mode is technically similar to the micromapping mode, hence some settings are similar:

* `kinds` (or `kind`, a string or a list of strings): objects that are edited in this mode. `amenity` by default.
* `otherKinds` (or `otherKind`): objects that are shown as black dots on the map. `micro` by default (yes, the opposite of the Micromapping set up).
* `defaultPresets` (a list of strings): preset names for the list appearing after you tap the (+) button.

But there are new ones:

* `checkIntervals`: a mapping of day intervals for various element kinds: how often one could return and confirm the amenity by pressing a ✓ checkmark in the tile. By default it has two entries: `structure: 360` and `amenity: 60`. The latter is a catch-all, it's not exactly matched against the `amenity` kind.

Currently you cannot create new instances of this mode.

### Entrances

This mode can be duplicated. Actually, it cannot be modified at the moment, since there are no replaceable attributes, so copying is the only mode of operation.

Fields for a new mode:

* `kinds`: list of element kinds this mode supports.
* `primary`: a mapping for setting up the primary type button (bottom right for the right-hand operation):
    * `preset`: a preset name that's the editor is open with.
    * `icon`: what's drawn on the button.
    * `tooltip`: a tooltip for the icon.
    * `adjustZoom`: if different from 0.0, when you start dragging the button onto the map, it temporarily changes zoom level inwards. For example, the default entrances button has this set to 0.7.
* `secondary`: the same for the second button.
* `rendering`: a mapping for element kind → rendering definition. Each one can have those keys:
    * `requiredKeys`: list of tag keys that need to be non-empty in order for the object to be considered fully surveyed. For example, a tree could have `[leaf_type, leaf_cycle]`.
    * `icon`: whether to draw an icon here. Add `iconPartial` to notice missing keys.
    * `label`: a template to print on the marker, e.g. `{capacity}` for bicycle parking. Keys should be enclosed in figure quotes.
    * If nothing is specified, a round marker would be used, filled yellow for "complete" and white otherwise.

### Notes

Currently cannot be customized much, and because of this, cannot be duplicated. You can only change one property of the `notes` mode:

* `locked`: whether the drawing is locked when you open the mode screen. Note that it changes the state at the moment of plugin loading, but does not persist when the app is active. Hence it affects only the initial state when the app is open.
