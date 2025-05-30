---
title: Element Kinds
---
An "element kind" is a set of rules that apply to tags to split objects into groups. Before the refactoring, some groups were made to non-intersect, but that can be untrue now. Each element kind has a string name, and a set of tag matching rules.

We have multiple pre-defined element kinds:

* `amenity`: objects that are editable in the "Amenity" mode. Usually they have opening hours, and deliver some services. Almost always it's for money. Shops, offices, parcel points, ATMs go here.
* `needsCheck`: default to the `amenity` kind, indicates if the object benefits from regular assessment. Manifests as a checkmark in the POI editor mode.
* `structure`: a subset of amenities that usually exist for decades or centuries once built. Examples are schools, churches, and stadiums.
* `micro`: objects in the micromapping mode. Defined as "not amenity", and with a list of allowed keys.
* `needsInfo`: lists of important missing tags for micromapping-related objects.
* `building`: basically `building=*`, except for `building=entrance`.
* `entrance`: basically `entrance=*` or `building=entrance`.
* `address`: an address node. It should have no other use (no "main tag"), and either `addr:housenumber` or `addr:housename`.
* `everything`: every key-value combination by which objects are filtered from the downloaded OSM extract. A rule of thumb is, no linear feature tags, and no polygons that are usually too big to grasp from being there.
* `empty`: no meaningful tags except `source` and `note`.
* `unknown`: a nothing-kind that's returned when an object does not match other kinds.

## Main Key

Before everything else, we need to clarify what "main key" means. Every Door works on an assumption that every object in OSM data represents one separate entity. This is not always correct (e.g. shops on buildings, or fences and school areas), but mostly checks out: it is exceptionally rare to find a `shop` and `amenity` on a single POI object.

To detect the main key, Every Door iterates over [a list of keys](https://github.com/Zverik/every_door/blob/main/lib/helpers/tags/main_key.dart), taking the first one found. So for a polygon with `amenity=school` and `barrier=fence`, it would choose the first one. It does consider [lifecycle prefixes](https://wiki.openstreetmap.org/wiki/Lifecycle_prefix), so changing `shop` to `disused:shop` changes nothing with regard to the main key.

It is not possible to extend the main key list through a plugin, you would need to submit a pull request.

Now, some standard element kinds apply only to the main tag (key and value). Those are `amenity`, `needsCheck`, `structure`, `micro`, and `everything`. Others either don't care for an object type (like `empty`), or acknowledge an object can be multiple things at once (like `building`).

## Defining Kinds

A simple `ElementKind` looks like this:

```yaml
kinds:
  cycleBarrier:
    matcher:
      barrier:
        only: [cycle_barrier]
```

The `cycleBarrier` element kind matches only a single tag, `barrier=cycle_barrier`, and only as a main key. If an object has more important defining tag, like `amenity=post_box`, the matcher won't pick up the barrier tag.

To look for matches in every tag, you can set `onMainKey: false` for the element kind definition. As mentioned earlier, it is rarely needed, so you must know what you're doing.

The only other thing in a kind is a tag matcher. It is defined in the `matcher` key when we're defining a new element kind, or completely replacing an existing one. For updating an existing element kind, e.g. for adding a support for a new tag, use instead the `update` key:

```yaml
kinds:
  everything:
    update:
      amenity:
        replace: true
        except:
          - parking
          - parking_entrance
          - loading_dock
          - waste_dump_site
          - waste_transfer_station
```

You might think, this looks exactly like the [current definition](https://github.com/Zverik/every_door/blob/main/lib/helpers/tags/element_kind_std.dart#L306-L313) of the `amenity` tag handling, but here we skipped the `parking_space` value, allowing for editing its properties.

Note that the editor won't touch linear features, so there is no point in enabling, for example, `waterway=river` or `highway=residential`.

## Matchers

A tag matcher (the one you specify for an element kind) looks at tag keys, like `amenity` or `man_made`. It's the first layer of testing: if a key is not in its dictionary, it fails the check.

The definition for a matcher is a map with tag keys for keys, and matching rules for values. Those matching rules can have those keys:

* `only`: a list of values that can match. If it's not empty, all other values or an absence of the key would fail the check.
* `except`: a list of forbidden values. The check will fail if the tag is present, and its value is in this list.
* `when`: a map of "value: tag matcher". It invokes a nested matcher when the tag has the given value. It allows for testing secondary tags, e.g. `recycling_type=*` for `amenity=recycling`.
* `replace`: when redefining a matcher, this boolean flag (`true` by default) controls whether we're replacing all the rules, or updating the lists. Matchers in `when` get replaced in any case. Note that you cannot _remove_ entries from `only` and `except`, only append to the lists. To remove, just copy a list without some entries, like we did above in the `amenity=parking_space` example.

### Matching on the Main Key

If an element kind containing tag matcher works on main keys, the matcher can automatically accept an object when certain keys are present. A list of those keys goes into `$good` attribute. It looks like this:

```yaml
kinds:
  amenityKind:
    matcher:
      "$good":
        - shop
        - craft
        - office
        - healthcare
        - club
      amenity:
        except: [parking, ...]
        when:
          recycling:
            recycling_type:
              only: [centre]
```

This example illustrates both keys that are considered amenities for every possible values, and a conditional `amenity=recycling` handling.

### Missing Tags

There is also a `$missing` tag matcher list, which behaves weirdly. It passes only when at least one of the listed keys are missing. That is, if we have `"$missing": [leaf_type, leaf_cycle]`, then the matched will pass only when the object does not have at least one of those two tags.

Only one element kind uses the `$missing` list, and it's `needsInfo`. The micromapping mode uses it to display a white dot on objects that miss some important information. You can add that indication to new types of objects (based on their main key) using this example of how it's done for bus stops and pedestrian crossings:

```yaml
kinds:
  needsInfo:
    matcher:
      highway:
        crossing:
          when:
            "$missing": [crossing]
        bus_stop:
          when:
            "$missing":
              - bench
              - shelter
```
