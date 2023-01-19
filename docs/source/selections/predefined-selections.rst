Pre-defined selections
======================

The following selections are available in this library, divided in separate namespaces according to which subdetector they focus on.

Trigger
-------

.. list-table:: Trigger selections
   :widths: 25 50 25
   :header-rows: 1

   * - Name
     - Description
     - Parameters
   * - HasPhysicsTrigger
     - Check if the event is triggered by a physics trigger
     - None
  
HasPhysicsTrigger
^^^^^^^^^^^^^^^^^

This selection simply returns the value of ``NAIA::EventSummary::IsPhysicsTrigger()``


Tof
---

.. list-table:: Tof selections
   :widths: 25 50 25
   :header-rows: 1

   * - Name
     - Description
     - Parameters
   * - BetaInRange
     - Check if the Tof beta measurement is in a given range
     - Range lower bound, Range upper bound, Beta measurement type
   * - ChargeInRange
     - Check if the Tof charge measurement is in a given range
     - Range lower bound, Range upper bound, Charge measurement type
   * - GoodPathlength
     - Check if a given combination of Tof layers have a well defined track pathlength
     - A 4-bit bitset encoding the layer pattern to be checked 

BetaInRange
^^^^^^^^^^^

This selection checks if the Tof beta is within a specified range. You can specify the beta range, as well as the ``BetaType`` to check. 


ChargeInRange
^^^^^^^^^^^^^

This selection checks if the Tof charge is within a specified range. You can specify the charge range, as well as the ``ChargeType`` to check. 


GoodPathlength
^^^^^^^^^^^^^^

This selection checks if the Tof layers have a well defined pathlength. This means that either the Tracker track (or the Tof linear interpolation based
on the clusters position) crosses the Tof paddles from top to bottom (and doesn't intercept the sides). You can specify which layers should be checked by
passing a 4-bit bitset when creating the selection.

The 4 bits are associated with Tof layers in order from least significant to most significant (or right-to-left, if you will), meaning that if you want to 
check only Upper Tof layers the corresponding value is ``0b0011``, while for Lower Tof is ``0b1100`` (and ``0b1111`` for all 4 layers).  
