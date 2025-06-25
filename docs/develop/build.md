# Development Environment

## Installing Everything

Every Door is a Flutter app, and I cannot beat the conscise step-by-step
[setting up walkthrough](https://docs.flutter.dev/get-started/install) on the Flutter
website. Please follow it through.

My IDE of choice is the standard Android Studio: it includes an Android emulator
and all tools to build and package Android apps. On macOS it would make sense
to use XCode instead. There's no point in downloading VSCode just to install Flutter:
the alternative is just unzipping a file and adding the result to `PATH`.

For small changes I use Vim with
[YouCompleteMe](https://github.com/ycm-core/YouCompleteMe) for LSP
([see here](https://github.com/ycm-core/lsp-examples))
and [dart-vim-plugin](https://github.com/dart-lang/dart-vim-plugin) for formatting.
Note that Dart LSP is huge and slow to load, so there isn't much memory benefit from
using Vim over other IDEs.

Also keep in mind when installing Flutter SDK that despite it being 2025, it is still
risky using directories with spaces in them.

## Building Every Door

First, clone the repository:

    git clone https://github.com/Zverik/every_door.git

You can add `--recursive` if you plan on testing vendored Flutter SDK, but there's generally
no need.

Then you would need a preset database asset. For releases, it is
[downloaded](https://textual.ru/presets.db). Put it in the `assets/` directory.
For developing, the proper way would be to download
[taginfo-db.db](https://taginfo.openstreetmap.org/download) and unpack it somewhere (it's ~9 GB).
Then enter the `tools` directory and run:

    ./update.sh /path/to/taginfo.db

Now you need to create an language file `lib/l10n/app_zh.arb` with no content (`{}`),
because Flutter i18n has problems with absent parent language packs:

    echo '{}' > lib/l10n/app_zh.arb

And you're all set! Open the project in your IDE and run it, or build it from console:

```sh
flutter pub get
flutter build
flutter run # if you have a phone connected or an emulator running
```

## Submitting Changes

_Before_ starting work on a large feature, please consult with [design principles](design.md),
or maybe [contact Ilya](mailto:ilya@zverev.info). It would be a pity to leave a big pull request
rejected because of a disagreement. Of course that hasn't happened yet, and we strive to
accept all contributions.

If you're looking for a task to work on, check [the roadmap](https://github.com/users/Zverik/projects/1).
Anything on it is a wanted feature or a bug fix. An assigned milestone means higher priority.

Now, creating a pull request is pretty easy, you don't need to collect much data for it,
unlike a bug report. If the change is visual, a screenshot or a video would be appreciated.
Please keep changes to a minimum: e.g. don't reformat the entire code base. Note that we
use the standard default Dart format rules.

The one rule to have your changes merged is, **please respond to comments** and update
your code accordingly. We would like you to do all the needed changes, not do that ourselves.
Please rebase your branch onto `main` yourself if there are possible merge conflicts.
Unless there are conceptual issues with a pull request, and if you keep replying, your
work won't get lost, and you will be mentioned in a change log.

It's possible maintaner(s) might forget about your pull requests. Please wait a few days
and post a comment to your pull request to remind them. Every Door is a hobby project,
and maintainers might be busy with their jobs or family. Help in reviewing other pull
requests is appreciated!

Every Door is licensed under the ISC License. You should make every effort to ensure
you only submit patches which are unencumbered by conflicting intellectual property rights.
