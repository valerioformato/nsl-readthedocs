Defining a custom selection
===========================

Sometimes the selections provided by the library will not be enough for your analysis needs. In such cases you have the possibility of defining your own selections within the framework of this library, and have it combined with other selections as shown in previous examples.

The ``Selection`` class
-----------------------

Every selection already defined in this library derives from a common base class ``NSL::Selection``. The ``Selection.h`` header is not for the faint of heart so let's say immediately that the main thing you care about this class is that it defines the base interface for querying selections

.. code-block:: cpp

    // Selection.h

    // ...

    virtual bool operator()(Event &ev) { return (*m_matcher)(ev); };

which is the primary way you interact with any selection. The rest of the header is magic that makes the logical combination of different selections work efficiently.
But this little snippet of code shows something very important about the inner workings of ``NSL::Selection`` objects: they are not the ones performing the actual check. Instead, that operation is delegated to a specific object called ``matcher``

The ``matcher`` class
---------------------

A ``matcher`` is the real unit that checks properties of an event and decides wether the selection is passed or not. It works with the same exact interface of ``NSL::Selection``, by taking a ``NAIA::Event &`` and returning a ``bool``.

There are several different ``matcher`` types, but if you are writing a custom selection you will only be interested to the ``boolMatcher``, which is the object you will be using to initialize the private ``m_matcher`` member of your ``NSL::Selection`` derived class. Let's look at an example from the predefined selections and walk through it:

.. code-block:: cpp
    // NSL namespaces removed for the sake of clarity

    // BetaInRange.h
    class BetaInRange : public Selection {
    public:
      BetaInRange(float min, float max, NAIA::Tof::BetaType type);
    };

    // BetaInRange.cpp
    BetaInRange::BetaInRange(float min, float max, NAIA::Tof::BetaType type) {
      m_matcher = std::make_shared<boolMatcher>([=](Event &event) {
        auto charge = event.tofBase->Beta[type];
        return (charge > min && charge < max);
      });
    }

* In our header we define a new class for our selection, and we derive it from ``NSL::Selection``
* We define a constructor where we take in all the necessary parameters we might need
* In the constructor implementation we create a ``std::shared_ptr<boolMatcher>`` and assign it to the internal ``m_matcher`` member of our class
* We express the check we want to perform as a lambda function and pass it in as an argument when we create the ``boolMatcher`` object. Note that any parameter needed by the selection logic is captured by copy in our lambda.

The ``boolMatcher`` objects can be created from any functor or function object that can be converted into a ``std::function<bool(Event &)>``.

.. note::

   There are other types of ``matcher`` objects, but those are required to build logical combinations of selections. For example there is a ``andMatcher`` that takes two ``matcher`` and returns the logical AND of their results, and ``orMatcher`` for the logical OR, and so on.

   This is also why selections store a ``std::shared_ptr<matcher>``, so that any compound ``matcher`` can safely keep reference of the two ``matcher`` objects it needs to evaluate without copying them.
