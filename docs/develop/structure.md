# Code Navigation

The directory structure has been devised many years ago when the author has been learning
Flutter development, so it's not the most effective, but it works. This page walks you through
it, so you understand what goes where.

## Root

* `android`: Android build files go here. Of most importance are `app/build.gradle`
    and `app/src/main/AndroidManifest.xml`. If you don't know why, please stay away from those.
    * Built apk and aab files are put into `android/app/prod/release`.
    * Note that published apk's on Github use Ilya's personal certificate,
        while all beta apk's, including those published to Releases,
        are built with a secret certificate for Actions.
* `ios`: iOS build files. In XCode, you open `Runner.xcworkspace`.
    * Built distribution package is put into `build/ios/archive/Runner.xcarchive`.
        Open it with XCode to upload it to AppStore.
* `assets`: files that are packaged with the app. Mostly presets and images.
    * The production version of presets is [uploaded to Textual](https://textual.ru/presets.db).
* `vendor`: here are git submodules for building. Currently just Flutter SDK.
* `metadata`: text files for F-Droid, translated on Weblate.
    Might be good to go full Fastlane.
* `tools`: scripts in Python, Dart, and Bash to process data for the editor.
* `test`: yup, tests. Currently mostly unit tests for algorithmic helpers.
* `lib`: code.
* `pubspec.yaml`: The most important file for a Dart project, lists dependencies and stuff.

## Tools

* `common_hours.py`: the opening hours panel offers common starting and endind times.
    This script find those in the TagInfo database.
* `encode_url.dart`: encodes URLs with keys for imagery constants.
* `print_common_keys.py`: this script prepares the `lib/helpers/tags/common_keys.dart`,
    used in the raw tag editor.
* `test_sqlite_tags.py`: preset queries are pretty complex (see `lib/providers/presets.dart`),
    so this script was used to test those.

### Language Codes

* `language_names.py`: I've extracted the table from
    [this Wikipedia page](https://simple.wikipedia.org/wiki/List_of_ISO_639-1_codes)
    to a CSV (`language_codes.csv`), and extracted language names and ISO codes to a Dart constant.
* `match_country_langs.py`: this script build a reference table for languages in each
    country. The Unicode CLDR data might have been better, but this one uses
    [Organic Maps data](https://github.com/organicmaps/organicmaps/blob/master/data/countries_meta.txt).

### Building the Database

In short, run `update.sh` passing it a path to `taginfo-db.db` and wait.
It does five things:

1. Creates a Python virtual environment to run other scripts.
2. Runs `json_to_sqlite.py`: it downloads or reads distribution files from
    two repositories, [Tagging Schema](https://github.com/openstreetmap/id-tagging-schema/tree/main/dist)
    and [NSI](https://github.com/osmlab/name-suggestion-index/tree/main/dist),
    and repackages the info into an SQLite database. All preset processing,
    translations, and search indices are built there.
3. Runs `add_taginfo.py`. It requests keys for combo fields from this
    SQLite database, then runs queries on the TagInfo database (which is
    very big, so the queries are slow) to get the most popular values
    for those keys, and store them into presets.
4. Runs `add_imagery.py`. It stores the JSON data from the
    [Layer Index](https://github.com/osmlab/editor-layer-index/)
    repository in the SQLite database almost 1:1. Coverage polygons are
    converted into a geohash lookup table.
5. Packages NSI polygon collection into a constant string inside the code.

## Lib

* `main.dart`: the main executable file for the app. Contains the `MaterialApp`
    with necessary wrappers. Logging, colors, and Let's Encrypt certificate
    are all set up here.
* `constants.dart`: all the constants for various parts of the app. Including
    the app title and version, and API endpoints.
* `l10n`: localization `arb` files, translated with Weblate. The `app_en.arb`
    is the source and the only one you should edit.
    * During the build phase, those are compiled into `lib/generated`.

There is also `commit_info.g.dart` generated into the `lib` root with the
[commit\_info](https://pub.dev/packages/commit_info) package.

### Models and Controllers

* `models`: classes that are passed between the parts. The most important one
    is in `amenity.dart` with the POI definition. Everything editable is
    an `OsmChange`. Also note `field.dart` with two long functions to
    create a field from a preset record.
* `helpers`: non-visual functions and classes, and sometimes dictionaries.
    Most algorithmic stuff goes here, and into providers too. Frankly it's
    a mess, there's even a provider in there (`legend.dart`).
    * `geometry`: geometric and geodetic functions, e.g. distance calculation,
        or snapping points to lines.
    * `tags`: everything related to tags and tag filtering. Some of it is
        [described here](../plugins/metadata/element_kinds.md).
* `providers`: all the [Riverpod](https://riverpod.dev/) providers. They
    basically keep an application-wide state, and often are used for a
    pure storage (tied to an app context). Some providers (like `uploader.dart`)
    do not have a state, but link multiple providers together for convenience.

### Views

* `screens`: all the panes from the app. The app starts with the `loading.dart`,
    then switches to `browser.dart`, which displays the current mode page.
    * `modes`: mode pages that rely on corresponding `definitions`. The idea
        being, you can override a definition to create or modify a mode.
        There are four base modes, plus `navigate.dart` when you zoom out
        too far.
    * `editor`: panes and sheets used not only from the editor pane, but
        also from modes. E.g. `building` and `entrance` are displayed when
        you tap a button in the entrances mode. They all edit an `OsmChange`
        or display information related to it.
    * `settings`: all the panes accessible from the Settings menu.
* `widgets`: building blocks for the app: buttons, markers, info blocks,
    forms. The biggest thing is `map.dart` with a universal map widget.
* `fields`: classes inheriting `PresetField` that implement editors for
    various field types. Custom fields should go here.
    * `hours`: the opening hours editor is the most complex in the app,
        and all its parts are here.
    * `helpers`: helpers and panes that some fields open, e.g. a direction
        chooser with a map.

### Data

Not exactly about the code, but the important thing to understand is, where the
data is stored. In the app, it is distributed among four sources:

1. Android secure storage and shared preferences. Mostly for settings and tokens.
    For example, switches state from the Settings panel is stored there.
2. Presets SQLite database. It is packaged with the app and is read-only. It is
    accessed solely through `PresetProvider`.
3. App SQLite database. Mostly for downloaded OSM data, but also for location-dependent
    indices and settings, e.g. payment tags. See the `DatabaseHelper` for the schema
    and access.
4. Transient in-memory storage, in virtually every provider. Mostly caches.
