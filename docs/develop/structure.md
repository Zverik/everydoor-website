# Code Navigation

The directory structure has been devised many years ago when the author has been learning
Flutter development, so it's not the most effective, but it works. This page walks you through
it, so you understand what goes where.

## Root

* `android`: Android build files go here. Of most importance are `app/build.gradle`
    and `app/src/main/AndroidManifest.xml`. If you don't know why, please stay away from those.
* `ios`: iOS build files. In XCode, you open `Runner.xcworkspace`.
* `assets`: files that are packaged with the app. Mostly presets and images.
* `vendor`: here are git submodules for building. Currently just Flutter SDK.
* `metadata`: text files for F-Droid. Might be good to go full Fastlane.
* `tools`: scripts in Python, Dart, and Bash to process data for the editor.
* `test`: yup, tests.
* `lib`: code. The rest of this article (except for "Tools") covers it.
* `pubspec.yaml`: The most important file for a Dart project, lists dependencies and stuff.

## Tools

* `common_hours.py`
* `encode_url.dart`
* `language_names.py`
* `match_country_langs.py`
* `print_common_keys.py`
* `test_sqlite_tags.py`

### Building the Database

* `add_imagery.py`
* `add_taginfo.py`
* `json_to_sqlite.py`
* `update.sh`

## Models
