# Design Principles

Every Door is a highly opinionated editor. Which means, it makes a lot of assumptions
on behalf of the user, and pushes for a few optimized workflows. Core principles are
described in the [readme](https://github.com/Zverik/every_door/blob/main/README.md#design):

1. ED displays and edits only tagged nodes and polygons represented with their centerpoints. No roads.
2. ED focuses on surveying: adding and detailing things that you can see around you. Not map maintenance.
3. Fewer buttons and menus: heuristic is preferable to a setting, and every button benefits the surveyor.

Those principles can be circumvented with plugins, but they regulate the core experience.

## Data Complexity

The first principle says: we're editing only points. There are multiple reasons for this:

1. Easier to process data and relations between objects.
2. The screen is small, editing geometry would complicate everything while being inferior to desktop editing (and we have Vespucci).
3. We can customize markers for every goal, not so much lines and polygons.

Not having geometry restricts what you can do. For example, no drawing building contours or
setting addresses to enclosed amenities. No detecting nearest streets and no painting
downloaded OpenStreetMap data. All you have from OSM is a `OsmElement` object that has
a point location. That's by design.

If you think that to be limiting, consider also options for circumventing that. Like,
instead of tracing streets (from imagery), you have a free-form drawing in the Notes mode,
less consequential and hence more freeing.

OpenStreetMap data model is unfortunately too complex in regard to object interdependencies.
Adding `way` and `relation` processing would require support for splitting, adjusting,
straightening, and also correctly processing restriction and route relations. As a result,
we would have the second Vespucci or Go Map, and that's not the goal.

Instead, we are focusing on representation and _hiding_ the data model. Users edit not nodes
and tags, but buildings and shops. That's why we have multiple modes: while pedestrian crossings
and building entrances are denoted by nodes in ways, they belong to totally different classes
and need to be considered differently. We are thinking in terms, not what is possible to do,
but what exactly and with which purpose a user wants to do something. And from that, we
devise a user interface.

## Screen Estate

Most phones are smaller than a computer screen. Actually, often we have a 5" or 6" rectangle.
How do we fit an entire geospatial editor inside? QGIS has 74 buttons by default just on the
toolbar. JOSM has ~40 on two, plus multiple side panels. iD looks lean: it only has 20 buttons
and one permanent sidebar.
That is too much context one has to keep in their head, and too much controls for a small screen.

Every Door does not approach this as an excercise of hiding things behind other things.
No infinitely deep menus like in OsmAnd, no buttons behind buttons like in OsmTracker.
It's not a general purpose editor, and that allows us to shrink focus, to allow for less
at any given time, and thus opening more of the screen to the task at hand.

The third principle says, instead of a switch, just choose a state based on the context.
Instead of a button, try finding an interaction that comes naturally and does not require
an always-on control. If an interactive control is not used in ten minutes of hardcore mapping,
it should go.

That's why every new thing on the screen has to be properly thought out, maybe gaining extra
functions and proving itself over months of field testing, before it goes into the app.
It has to be useful for every user, not in specific circumstances. For everything else,
we've got plugins.

Note that some people use tablets or rotate their phones sideways. Are screen elements
laid out effectively in the landscape orientation?

## Flow

Surveying should bring a mapper joy. They are walking around and with each tap on the screen,
they are improving the map. Nothing should stay in the way, so those sessions last
longer. The mapper is feeling productive, and the app supports and encourages them
to map more.

That is the flow, and StreetComplete would be the best example of riding it. No hard
choices, no mode switching, just an unending series of questions and answers. On the
other end would be general purpose mobile editors with the constant "wait what did I just
press" feeling.

Every Door strives to be the most effective editor, allowing to collect one shop per minute,
with all the details, or survey a village for addresses from a moving car. Nothing should
stand in the way or slow the mapper down. Like:

* an important field too far down the form.
* a list of options too long to scroll.
* the mode sometimes wrong, making you consider the state.
* a common action inaccessible for non-obvious reason.

Those are all issues I have encountered with Every Door, and we need to have less of those,
not add to the list. Everything should be "at the fingertips", lowering the time
a mapper stares at the screen in favor of walking around and finding more things to map.
