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

### OsmChange

First we must mention that the single most important class in the app is
[`OsmChange`](https://github.com/Zverik/every_door/blob/v6.0/lib/models/amenity.dart).
It defines a set of changes over the base `OsmElement` received from OpenStreetMap,
and a few methods to manipulate an object. For example, it acts as a `Map<String, String>`
for tags, but keeps the changes separate.

It can calculate an age based on the `check_date`
tag (but note that it's a bit inconsistent with the overridden values for the
`amenity` mode). There are methods to confirm an object (`check()`) and to toggle
its disused status. The one most important method that's used everywhere returns
full tags in a map: `getFullTags()`. It is of course cached.

### Detecting a Preset

Now, to learn whether we need to add and shuffle fields, we first need to detect
a preset. Let's look at `PoiEditorPage`'s state. In the `initState()`, the `amenity`
(the object we edit) is either copied or created anew, and
[`updatePreset()`](https://github.com/Zverik/every_door/blob/v6.0/lib/screens/editor.dart#L101)
is called. For an already existing object (or a newly created from an NSI preset),
we call [`getPresetForTags()`](https://github.com/Zverik/every_door/blob/v6.0/lib/providers/presets.dart#L231)
from a preset provider.

That method has got a lot of steps: checking for empty tags and `amenity=fixme` tags,
asks plugins for an answer. But the main step is running a huge SQL query against
the local SQLite database, which through clever CTE and sorting returns the one result.

Is it good to have a single big SQL query? Felt like a clever idea at the time, but
now I'm not so sure. But this method gets called dozens of times per second in the
micromapping mode, so this could be considered optimizing.

### Localization

All presets and fields are translated, so we pass the current `Locale` to preset
provider methods. The plugin subsystem too requires it — and that's basically it.
App localization is handled by the [intl](https://docs.flutter.dev/ui/accessibility-and-internationalization/internationalization)
package.

Passing locales to SQL queries in the preset provider works relatively simple.
We [create a CTE](https://github.com/Zverik/every_door/blob/v6.0/lib/providers/presets.dart#L92)
for a `langs` table that contains two columns, language
code and a rank, starting with 1. And we join the results with this table,
ordered by the language rank. Since window functions do not work in some
SQLite versions, we just query all the results and keep the first one for
each preset or field.

### Querying Fields

After we have a preset, we call [`getFields()`](https://github.com/Zverik/every_door/blob/v6.0/lib/providers/presets.dart#L460)
Initially a preset is returned with empty fields, because its name and title
can be all we need (e.g. for the micromapping mode). But for the editor, we need
the fields. And we do another SQL query to the presets database,
querying the `fields` table with the list from the `preset_fields`.

Besides checking for a location, sorting languages, and sorting into two
lists ("fields" and "more fields", based on the `required` flag), it does two
more things.

First is building the combo options. While needed only for combo fields,
it is a very resource-intensive operation, since it conflates three sources:

1. Options listed in the field definition.
2. Popular tag values from TagInfo.
3. Popular tag values from all the downloaded data.

The result is stored in a database table, to not do this again. But still, it is
another database query, so make it four for cache misses.

That's why the `getFields()` method implements its own in-memory caching. You
can notice how long the first loading of the editor pane takes. That's because
in the collapsed "more fields" section there are dozens of combo fields.

Finally, the method calls the long `fieldFromJson` function from
[`field.dart`](https://github.com/Zverik/every_door/blob/main/lib/models/field.dart).
It has some overrides for common keys, and for others, it uses the type.
It is pretty straightforward. When you're adding a field, do not forget
to create it _twice_ in this file, in both functions.

### Built-in Fields

### Sorting Fields

### Postcode and Opening Hours

### Standard Fields

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

Every Door uses [flutter\_map](https://pub.dev/packages/flutter_map) library for its mapping.
Over the years it's been doing pretty complex things with it (but not "going native" complex).
This section is mostly about providers and components: what happens when you change modes
implicitly.

_TODO_

1. Map controllers
2. Modes in the browser
3. Decisions on zoom levels and interactivity
