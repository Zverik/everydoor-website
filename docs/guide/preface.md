# Surveying for OpenStreetMap

To use Every Door efficiently and understand its design choices, you must first
learn what OpenStreetMap is, how data for it is collected, and why the app
was created in the first place. For that, we start with a bit of background.

## OpenStreetMap Basics

You must know this already: OSM has a very simple [data model](https://wiki.openstreetmap.org/wiki/Elements),
which consists of
tags, nodes, ways, and relations. Every entity can contain some number of any
previous entities: nodes have tags, ways have tags and nodes, and so on. Also
only nodes have any geometry, and areas can be represented as closed ways or
multipolygon relations.

The model simplicity leads to utter complexity of both editing the data and using it.
Loading it into a conventional data store means downloading an extract and
processing it with [Osmium](https://osmcode.org/) in some form. Clicking
through a geometry is simple — but choosing a correct tag is a pain. What suits better,
`natural=wood` or `landuse=forest`? Don't answer, it's a running joke.

In most editors the latter problem is circumvented with _presets_: a huge
mapping of real-world entities to OSM tags, each with a list of attribute tags.
It makes decisions simpler, but still does not solve all the issues: for
example, an object can be multiple things at once in OSM (like a fence and
a school ground), or have no meaning without a secondary tag (`amenity=recycling`
comes to mind).

### Points of Interest

All those issues become more apparent when you are adding a shop or a waste basket.
I'd recommend [watching this talk](https://www.youtube.com/watch?v=WQwO-vbGre0)
on the subject. To summarize it:

* Venues can be represented with points and polygons. Sometimes both.
* Finding a correct tag is often hard. There are many duplicates and nuances.
* Some shops have multiple types, while OSM allows for one.
* Even seemingly straightforward attributes can have subjective values.
* Addresses go on everything, not standardized, and often missing.
* Coverage is patchy, freshness unknown, mapping is hard.

In general, points of interest are the hardest thing to map, because there
are no good third-party sources. You have to go around and collect the data
yourself. And when you're back, you can spend hours on adding objects to the map,
and the result will be indistinguishable from before. Truly, tracing roads
and forests is much more gratifying — albeit possible to automate.

That's one reason mapping points of interest is important to OpenStreetMap, and
why this app in particular appeared. Another, there hasn't been any good ways
to keep the map fresh and collect data at speed.

## Sourcing the Data

Every Door comes from a decade of experiments with optimizing data collection.
Which is good, because it makes it the best in class, but also bad, because
high efficiency means steep learning curve. To understand those decisions, we
need to look back.

### Before Times

We used GPS loggers, pen and paper, and photo cameras. You walk around, take photos
or record all the shops or trees you see. When at home, you open JOSM, add a GPX
track from your logger, georeference pictures from the camera, put your walking
papers nearby, and start adding collected features one by one.

If it sounds tedious and super slow, well, it was. But the sense of power it brought
made us going.

_TODO_

### Apps

Editing map on a phone rids you of extra work when you get home. You just add everything
you see on the go, and then forget about it. Alas, mobile phones have different
issues: mainly, from the small screen size and imprecise controls.

_TODO_

## Unsolved Problems
