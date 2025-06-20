# Installation

Plugins are not meant to sit on your computer and do nothing. How to load them
onto your phone, and make them available to thousands of Every Door users (if
you want that)?

You have four options for that.

1. Share an `.edp` file. For example, send it to yourself with a messenger, and
    open it on a device. This way you don't have to publish it, and you can
    iterate on the development quickly. One issue is, it might not work on iOS.

2. Upload it to the [public repository](https://plugins.every-door.app/). It would
    validate the package on upload, reserve a plugin identifier, and make it available
    in the app. Just go to "Settings → Plugins → (+) button" and find it there.
    You only need an OpenStreetMap account to publish plugins. Initially you can
    mark your plugin "experimental", so it won't appear in public lists.

3. Upload it somewhere open, like to Github or your own web server. This option works
    best for programmatically built plugins, e.g. for mapping party settings.
    The file name should be equal to the plugin identifier.
    To share it, create a QR code with the link to the file, and scan it with the
    phone from the "Settings → Plugins → (+) → the button at the top right".

4. If some of your devices do not support reading QR codes, or you're publishing
    plugins on a website intended to be used from a phone, then construct a plugin
    installation link. It looks like this:

    `https://plugins.every-door.app/i/plugin_id?url=https%3A%2F%2Fexample.com%2F...%2Fplugin_id.edp`

    The `plugin_id` in the base url should match the file name and the identifier
    from `plugin.yaml`, but it would also match against the plugin repository list.
    You can use `my` instead, like `/i/my?...`.

    The link can also have query parameters: `version` to [versionize plugins](metadata/index.md#versioning-plugins)
    (useful when you update a published plugin), `update=true` to force an
    update even when a newer version of the plugin has been installed.

## Troubleshooting

If the installation fails, check the system log: press the button on screen,
or find those in "Settings → About → System Log".

To delete a plugin, open it in the settings panel, and tap "Delete".
