# Code Navigation

The directory structure has been devised many years ago when the author has been learning
Flutter development, so it's not the most effective, but it works. This page walks you through
it, so you understand what goes where.

## Root

* `android`: Android build files go here. Of most importance are `app/build.gradle`
    and `app/src/main/AndroidManifest.xml`. If you don't know why, please stay away from those.
    * Built apk and aab files are put into `android/app/prod/release`.
    * Note that published apk on Github use Ilya's personal certificate,
        while all beta apks, including those published to Releases,
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

* `common_hours.py`
* `encode_url.dart`
* `print_common_keys.py`
* `test_sqlite_tags.py`

### Language Codes

* `language_names.py`
* `match_country_langs.py`

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

* `models`:
* `helpers`:
* `providers`:

### Views

* `screens`:
* `widgets`:
* `fields`:
