# Developing and Translating Presets

The editor uses [presets from iD](https://github.com/openstreetmap/id-tagging-schema):
they are managed in a dedicated repository and translated on [Transifex](https://www.transifex.com/openstreetmap/id-editor/translate/#ru/presets/).

To translate value options, first make a pull request to the iD tagging repo
adding desired options, [like here](https://github.com/openstreetmap/id-tagging-schema/blob/main/data/fields/camera/type.json).
Then, when the translation source on Transifex is updated, there will be strings to translate.
[Like here](https://www.transifex.com/openstreetmap/id-editor/translate/#ru/presets/101711314?q=key%3Apresets.fields.camera%2Ftype).

Brands are managed in the [Name Suggestion Index](https://github.com/osmlab/name-suggestion-index).
