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
  
*HasPhysicsTrigger*
^^^^^^^^^^^^^^^^^^^

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

*BetaInRange*
^^^^^^^^^^^^^

This selection checks if the Tof beta is within a specified range. You can specify the beta range, as well as the ``BetaType`` to check. 


*ChargeInRange*
^^^^^^^^^^^^^^^

This selection checks if the Tof charge is within a specified range. You can specify the charge range, as well as the ``ChargeType`` to check. 


*GoodPathlength*
^^^^^^^^^^^^^^^^

This selection checks if the Tof layers have a well defined pathlength. This means that either the Tracker track (or the Tof linear interpolation based
on the clusters position) crosses the Tof paddles from top to bottom (and doesn't intercept the sides). You can specify which layers should be checked by
passing a 4-bit bitset when creating the selection.

The 4 bits are associated with Tof layers in order from least significant to most significant (or right-to-left, if you will), meaning that if you want to 
check only Upper Tof layers the corresponding value is ``0b0011``, while for Lower Tof is ``0b1100`` (and ``0b1111`` for all 4 layers).  


InnerTracker
------------

.. list-table:: Tracker selections
   :widths: 25 50 25
   :header-rows: 1

   * - Name
     - Description
     - Parameters
   * - ChargeInRange
     - Check if the Inner Tracker charge measurement is in a given range
     - Range lower bound, Range upper bound, Charge measurement type
   * - ChargeRMSLessThan
     - Check if the Inner Tracker charge RMS is less than a given value
     - Upper bound, Charge measurement type
   * - HitPattern
     - Check if the track hits satisfy the canonical ``L2 && (L3 || L4) && (L5 || L6) && (L7 || L8)`` pattern
     - None
   * - NHitsGreaterThan
     - Check if the track has at least a minimum number of hits on a given side 
     - Lower bound, Tracker side
   * - NGoodClustersGreaterThan
     - Check if the track has at least a minimum number of hits with a good charge status 
     - Lower bound, Cluster status mask

*ChargeInRange*
^^^^^^^^^^^^^^^^

This selection checks if the Inner Tracker charge is within a specified range. You can specify the charge range, as well as the 
``ChargeRecoType`` to check. 

*ChargeRMSLessThan*
^^^^^^^^^^^^^^^^^^^

This selection checks if the Inner Tracker charge RMS is lower than a specified value. You can specify the upper limit, as well as 
the ``ChargeRecoType`` to check. 

*HitPattern*
^^^^^^^^^^^^

This selection checks if the Inner Tracker hits satisfy the canonical ``L2 && (L3 || L4) && (L5 || L6) && (L7 || L8)`` pattern. 

*NHitsGreaterThan*
^^^^^^^^^^^^^^^^^^

This selection checks if the Inner Tracker track has at least a minimum number of hits on a given side. You can specify the lower limit of hits, 
as well as the Tracker side to consider.

*NGoodClustersGreaterThan*
^^^^^^^^^^^^^^^^^^^^^^^^^^

This selection checks if the Inner Tracker track has at least a minimum number of clusters with a good status on a given side. You can specify the 
lower limit of hits, as well as the mask to compare the hit status against (see 
`here <https://ams.cern.ch/AMS/Analysis/hpl3itp1/root02_v5/html/development/html/classTrClusterR.html#a24ef522472bd83d45174daee1f1853a9>`_ for 
details on the charge status word).   