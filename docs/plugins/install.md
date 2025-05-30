# Installation

You would need to make your plugin available from some URL.

Look for the "Plugins" entry in the Settings pane (behind the top left stripes button). At the top right there will be a button to scan a QR code of an URL. Alternatively, you could form an URL like such (it's a working [URL](https://every-door.app/plugin/cycling?url=https%3A%2F%2Ftextual.ru%2Fcycling.edp&update=true), "cycling" is the plugin identifier):

    https://every-door.app/plugin/cycling?url=https%3A%2F%2Ftextual.ru%2Fcycling.edp&update=true

The plugin file should have an `.edp` file extension, and be a simple ZIP archive with a `plugin.yaml` file at the top level. Alas this method is not yet stable, QR codes are easier.

Here is a QR code for you to test on:

![](cycling_edp.png){ width="200" }

An alternative QR code for a [plugin](https://every-door.app/plugin/cycling?url=https%3A%2F%2Ftextual.ru%2Fimagery.edp&update=true) that adds some imagery for the Nõmme region of Tallinn, Estonia, is behind this QR code:

![](imagery_edp.png){ width="200" }

If the installation fails, check the system log in "Settings → About → System Log".

Installation from sharing an archive file is not yet supported. It would require switching `app_links` package with `receive_sharing_intent`, and that's already too much work. We'll postpone this to the next stage.

To delete a plugin, swipe it to the left, just like with the edits.
