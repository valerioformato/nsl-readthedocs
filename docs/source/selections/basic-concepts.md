# Basic concepts

## What is a selection?

Almost every time we analyze a set of events we are only intereseted in a precise subset of them, and usually
we filter out events we don't care about by asking simple yes/no questions about the value of some variables.

So, in short, a selection is just a yes/no question about variables in an event. This whole library is based around this concept.

Whenever you instantiate a `NSL::Selection` object you can always query wether a `NAIA::Event` passes that selection

```cpp
NSL::Selection my_selection = get_some_selection();

// ...
for (NAIA::Event &ev : chain) {
  if (my_selection(event)){
    // do stuff...
  }
}
```

That's simple enough and it is really at the core of this library. But things can get out of hands pretty soon because
usually we want to evaluate tens of selections before we decide an event is interesting enough to be in our analysis.
What we want to avoid is something like

```cpp
if (my_selection_1(event) && my_selection_2(event) && /* ... */ my_selection_N(event)) {
```

that would make code hard to read and mantain pretty quickly. Not to mention that the whole analysis logic is
conceptually defined inside the main even loop, even if it's likely to be always the same throughout the whole
executable.

This is the reason why another main point of this library is allowing to define the whole selection logic ahead
of time **before** the event loop, so that in the event loop you can focus on what you actually do with the events
you select, rather than how you select them.

It boils down to enabling something like

```cpp
NSL::Selection combined_sel = selection_1 && selection_2;
```

which means constructing a more complex selection as a logic combination of simpler selections and still have it
behave in the same way. This, combined with a wealthy library of already defined selections, should allow to more
easily share event selection between analysis and avoid the ever-present "reinventing the wheel" problem.

Let's look at an actual example:

```cpp
namespace ns = NSL::Selections;

auto triggerSel = ns::Trigger::HasPhysicsTrigger();

auto innerTrackerSel =
    ns::InnerTracker::HitPattern() &&
    ns::Track::ChiSquareLessThan(10.0f, NAIA::TrTrack::Side::Y, NAIA::TrTrack::Fit::GBL, NAIA::TrTrack::Span::InnerOnly);

for (NAIA::Event &ev : chain) {
  if (!triggerSel(event))
    continue;

  if (!innerTrackerSel(event))
    continue;

  // fill histos or whatever :)
}
```

```{note}
Uniformity and shared knowledge is generally a good thing, but comes at the expense of analysis independence.
This is a really important subject, but it's best discussed elsewhere :)
```

As shown in the previous example the most common selections types used in almost every analysis are already implemented and can be parametrized in order
to maintain flexibility. The existing selections should allow to express at least 90% of all "classic" analyses while the remaining 10% can be covered
defining custom selections. However this is a more advanced topic which is covered later on in this guide. 