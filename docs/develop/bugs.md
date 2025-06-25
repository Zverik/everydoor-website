# Bugs and Wishes

Every Door issues are collected and processed [on Github](https://github.com/Zverik/every_door/issues).
In order to submit or comment a report, you need to register and login. Just press "New Issue" and
fill in the form.

Mind the application part you're reporting about:

* If it's about an object type or fields on the editing panel, that is most likely about presets.
  See the [presets page](presets.md) about contributing.
* If something is wrong with a map layer underneath markers, you mind need to contact
  [OSM-Carto](https://github.com/gravitystorm/openstreetmap-carto/) map layer developers.
* If translations are incorrect, you most likely know how to improve them.
  [This guide](translate.md) will help you.

Before reporting, first open Settings (the three-line button at the top left) → About Every Door.
Make sure the version is the latest one available in app stores for your platform.

Also, try disabling all plugins (Settings → Plugins). If it helps, please
[report a plugin issue](#plugin-issues).

## Best Practices

When reporting an issue, please provide as much information as possible. It would help
if you include:

* Every Door version, platform, and from where you got the app.
* What you expected, and what you got instead. Possibly with reasons for your expectations.
* Relevant parts, or an entire system log, available in Settings → About → System Log.
* A screenshot or two.

Please do not be intimidated with the list: often the bug is pretty straighforward and
does not need many details to be triangulated. But the more details developers get,
the better.

One important thing is not to report multiple issues in a single ticket. Even if they are
tiny. This helps developers be productive and mentally healthy. Opening multiple tickets
is fine, please do not lump them into a single one.

## Asking for Features

Mapping varies from country to country, and it is entirely possible Every Door does not
have a feature that is important for surveyors somewhere. Feature requests are welcome!

With that, please take note of [design principles](design.md). Every Door is not a
general-purpose editor, and it cannot be everything. We have Vespucci and Go Map for that.
So please be prepared to explain your request deeper, and for it to possibly be rejected.
We do not close tickets without explanation, but sometimes it is impossible to cram
everything into a single app.

It is quite hard to determine how complex a feature is, so please be patient. Some
features may take years to be implemented. The editor is developed in free time as a hobby
project, so long development cycles are to be expected. Generally, the more fun or useful your
request is, the eager developers are to work on it. Alas usefulness does not equate fun,
and developers have a weird definition of "fun", so it's nearly impossible to predict. Sorry.

You may try to create your feature [as a plugin](../plugins/index.md) instead.

## Road Map

Every Door development is tracked in [this project](https://github.com/users/Zverik/projects/1).
It's split by milestones (that is, versions). It is unlikely that your bug or a feature
would get worked on if they are not assigned a milestone, or do not have any labels.

The road map should not be taken as a firm list of features that will be released with
the new app version. It mostly sorts tickets into priorities: which should be worked on
now, and which could be postponed when maintainers have more time, or a developers comes
eager to work on something.

## Issues With Websites

This documentation website (which doubles as a landing page) is also maintained
[on Github](https://github.com/Zverik/everydoor-website). You technically can create
issues there. Maybe I have missed something important in guides, so I'd be happy
to be reminded. Obviously you can create a pull request instead.

The plugin repository source code [is here](https://github.com/Zverik/everydoor-plugin-repo).
It is pretty raw, so bugs are to be expected. Please create issues in that repo,
not in Every Door's. Code is also welcome.

## Plugin Issues

Alas we cannot fix any issues with plugins, regardless of where they came from.
One exception is when you are a plugin author, and the issue actually comes from Every Door.
But that case has been outlined above.

You have two options for reporting a plugin issue:

1. On the plugin repository page for that plugin, there might be a link to the plugin
    home page (which might be a Github repository). That page would direct you to means
    of reporting issues.
2. Since only OpenStreetMap users can upload plugins, the author's name there is actually
    an OSM display name, for which you can search. Try writing them a private message
    via the OSM website.

As a last resort, you might try creating a topic on the OpenStreetMap community forum.
Somebody might help you with your problem then. But alas no guarantees.
