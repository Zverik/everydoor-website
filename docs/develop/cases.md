# How To Find Things

The size of the code might be daunting (it is for me, the author), so this section
is intended to help with finding, where exactly to contribute when you want to change
something.

Note that the code is in a constant (but glacial) flux, so some of those might be
possible to change with a plugin in the future.

## Editing Standard Fields

As mentioned in the [plugin docs](../plugins/metadata/presets.md), "standard fields" is
a third group of fields that gets added on top of "fields" and "more fields" for every
object in the editor panel. This chapter traces its addition, marking what and where
can be improved.

_TODO_

1. OsmChange vs OsmElement
2. Detecting a preset
3. Querying fields
4. Localization
5. Fields from plugins
6. Built-in fields
7. Sorting fields
8. Postcode and opening hours

## Uploading to OpenStreetMap

This section traces what happens when a user taps the upload button, but does not explain
the entire uploading algorithm: it's a bit too complex. It would be useful to you to
understand how providers interact, and how the data is packaged.

_TODO_

1. Nav panel, upload button
2. Upload provider
3. Locking
4. Uploading OSM changes
    * Splitting changes
    * Downloading data
    * Uploading data
    * Updating the inner storage
5. Uploading notes
6. Uploading scribbles
7. Notification

## Zooming Out

Every Door uses [flutter_map](https://pub.dev/packages/flutter_map) library for its mapping.
Over the years it's been doing pretty complex things with it (but not "going native" complex).
This section is mostly about providers and components: what happens when you change modes
implicitly.

_TODO_

1. Map controllers
2. Modes in the browser
3. Decisions on zoom levels and interactivity
