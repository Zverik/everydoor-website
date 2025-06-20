## How To Build

You will need the [Flutter SDK](https://docs.flutter.dev/development/tools/sdk/overview) installed.
Alternatively, clone with submodules (`git clone --recursive`) and use `vendor/flutter/bin/flutter`. That
is the preferred way for releases.

1. Download [taginfo-db.db](https://taginfo.openstreetmap.org/download) and unpack it somewhere (it's ~9 GB).
2. From the `tools` directory, run `./update.sh <path_to_taginfo_db>`.
    * Alternatively, do `curl https://textual.ru/presets.db -o assets/presets.db`
3. `echo '{}' > lib/l10n/app_zh.arb` (fixing Dart's localization issues).
4. `flutter pub get`.
5. `flutter build`.
