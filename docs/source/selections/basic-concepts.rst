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
   for(NAIA::Event &ev : chain) {
     if( my_selection(event) ){
       // do stuff...
     }
   }

That's simple enough and it is really at the core of this library.
