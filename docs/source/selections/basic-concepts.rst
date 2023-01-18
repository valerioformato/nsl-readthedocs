Basic concepts
==============

What is a selection?
--------------------
Almost every time we analyze a set of events we are only intereseted in a precise subset of them, and usually
we filter out events we don't care about by asking simple yes/no questions about the value of some variables.

So, in short, a selection is just a yes/no question about variables in an event. This whole library is based around this concept.

Whenever you instantiate a ``NSL::Selection`` object you can always query wether a ``NAIA::Event`` passes that selection

.. code-block:: cpp

   NSL::Selection my_selection = get_some_selection();

   // ...
   for (NAIA::Event &ev : chain) {
     if (my_selection(event)){
       // do stuff...
     }
   }

That's simple enough and it is really at the core of this library. But things can get out of hands pretty soon because
usually we want to evaluate tens of selections before we decide an event is interesting enough to be in our analysis.
What we want to avoid is something like

.. code-block:: cpp

   if (my_selection_1(event) && my_selection_2(event) && /* ... */ my_selection_N(event)) {

that would make code hard to read and mantain pretty quickly. Not to mention that the whole analysis logic is
conceptually defined inside the main even loop, even if it's likely to be always the same throughout the whole
executable.

This is the reason why another main point of this library is allowing to define the whole selection logic ahead
of time **before** the event loop, so that in the event loop you can focus on what you actually do with the events
you select, rather than how you select them.

It boils down to enable something like

.. code-block:: cpp

   NSL::Selection combined_sel = selection_1 && selection_2;

which means constructing a more complex selection as a logic combination of simpler selections and still have it
behave in the same way. Let's look at an actual example:

.. code-block:: cpp

   namespace ns = NSL::Selections;

   auto triggerSel = ns::Trigger::HasPhysicsTrigger();

   auto innerTrackerSel =
       ns::InnerTracker::HitPattern() &&
       ns::Track::ChiSquareLessThan(10.0f, NAIA::TrTrack::Side::Y, fitType, NAIA::TrTrack::Span::InnerOnly);

   for (NAIA::Event &ev : chain) {
     if (!triggerSel(event))
       continue;

     if (!innerTrackerSel(event))
       continue;

     // fill histos or whatever :)
   }
