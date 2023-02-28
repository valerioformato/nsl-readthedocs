Pre-defined selections
======================

The following selections are available in this library, divided in separate namespaces according to which subdetector they focus on.

Trigger
-------

.. hint::
   *Under namespace* NSL::Selections::Trigger

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

.. hint::
   *Under namespace* NSL::Selections::Tof

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

.. hint:: 
  *Under namespace* NSL::Selections::InnerTracker

.. list-table:: Inner Tracker selections
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


Full Track
----------

.. hint::
   *Under namespace* NSL::Selections::Track

.. list-table:: Track-related selections
   :widths: 25 50 25
   :header-rows: 1

   * - Name
     - Description
     - Parameters
   * - ChiSquareLessThan
     - Check if the Track chi-square is less than a specified threshold, for a given side, fit and span.
     - Upper bound, tracked side, track fit algorithm, track span.
   * - InnerFiducialVolume
     - Check if the Track lies within a pre-defined fiducial volume defined on the individual tracker planes of the Inner Tracker.
     - Track fit algorithm, track span.
   * - L1FiducialVolume
     - Check if the Track lies within a pre-defined fiducial volume defined on the L1 tracker plane.
     - Track fit algorithm, track span.
   * - L9FiducialVolume
     - Check if the Track lies within a pre-defined fiducial volume defined on the L9 tracker plane.
     - Track fit algorithm, track span.
   * - HitCut
     - Check if the Track has a hit on a given tracker layer.
     - Layer J-number (1...9).
   * - L1NormResidualLessThan
     - Check if the normalized residual on L1 is below a specified threshold, for a given fit.
     - Upper bound, track fit algorithm.


*ChiSquareLessThan*
^^^^^^^^^^^^^^^^^^^

This selection checks if the track has chi-square per degree of freedom less than a specified threshold, on a given side. 
You need to specify a track fit algorithm and a track span.

*InnerFiducialVolume*
^^^^^^^^^^^^^^^^^^^^^

This selection checks if the track lies inside a pre-defined fiducial volume within the Inner Tracker. This fiducial volume is defined 
removing the most external part of the inner tracker layers. You need to specify a track fit algorithm and a track span.

.. list-table:: Fiducial volume definition
  :widths: 20 40 40
  :header-rows: 1

  * - Layer
    - R boundary
    - Y boundary
  * - L2
    - 62 cm
    - 40 cm
  * - L3 / L4
    - 46 cm
    - 44 cm
  * - L5 / L6
    - 62 cm
    - 36 cm
  * - L7 / L8
    - 46 cm
    - 44 cm

*L1FiducialVolume*
^^^^^^^^^^^^^^^^^^

This selection checks if the track lies inside a pre-defined fiducial volume within the L1 plane. This fiducial volume is defined 
removing the most external part of the L1 plane. You need to specify a track fit algorithm and a track span.

.. list-table:: Fiducial volume definition
  :widths: 20 40 40
  :header-rows: 1

  * - Layer
    - R boundary
    - Y boundary
  * - L1
    - 62 cm
    - 47 cm
  
*L9FiducialVolume*
^^^^^^^^^^^^^^^^^^

This selection checks if the track lies inside a pre-defined fiducial volume within the L9 plane. This fiducial volume is defined 
removing the most external part of the L1 plane. You need to specify a track fit algorithm and a track span.

.. list-table:: Fiducial volume definition
  :widths: 20 40 40
  :header-rows: 1

  * - Layer
    - R boundary
    - Y boundary
  * - L9
    - 43 cm
    - 29 cm

*HitCut*
^^^^^^^^

This selection checks if the Track has a hit on a given tracker layer (effectively equivalent to check ``TrTrackR::GetHitLJ`` 
`in gbatch <https://ams.cern.ch/AMS/Analysis/hpl3itp1/root02_v5/html/development/html/classTrTrackR.html#a82eeb22a1bb99dae96e4f364eb43d404>`_).

*L1NormResidualLessThan*
^^^^^^^^^^^^^^^^^^^^^^^^

Check if the normalized residual on L1, defined as
:math:`\chi^2_\text{IL1} \cdot \text{NDoF}_\text{IL1} - \chi^2_\text{Inner} \cdot \text{NDoF}_\text{Inner}`
, is below a specified threshold, for a given fit.


Tracker Layer Charges
---------------------

.. hint::
   *Under namespace* NSL::Selections::TrackerLayer

.. list-table:: Layer Charge-related selections
   :widths: 25 50 25
   :header-rows: 1

   * - Name
     - Description
     - Parameters
   * - ChargeInRange
     - Check if the Tracker charge (combination of X and Y measurements) of a particular layer is in a given range
     - Layer J-number (1...9), Range lower bound, Range upper bound, Charge measurement type
   * - ChargeStatus
     - Check if the Track cluster on a particular layer has a good status (checked using the default ``0x10013D`` mask)
     - Layer J-number (1...9)
   * - ChargeAsymmetry
     - Check if the relative difference between Tracker charge on X and Y views for a particular layer is below a specified threshold
     - Layer J-number (1...9), Upper bound, Charge measurement type

*LayerChargeInRange*
^^^^^^^^^^^^^^^^^^^^

This selection checks if the Tracker charge of a particular layer is in a given range. 
This check is based on the combined charge estimation from both X and Y sides. 
You can specify the charge range, as well as the ``ChargeRecoType`` to check. 

*LayerChargeInRange*
^^^^^^^^^^^^^^^^^^^^

This selection checks if the Tracker charge of a particular layer has a good status word.
(see `here <https://ams.cern.ch/AMS/Analysis/hpl3itp1/root02_v5/html/development/html/classTrClusterR.html#a24ef522472bd83d45174daee1f1853a9>`_ for 
details on the charge status word).

*LayerChargeAsymmetry*
^^^^^^^^^^^^^^^^^^^^^^

This selection checks if the Tracker charge asymmetry for a particular layer is below a specified threshold.
The asymmetry is defined as ::math:`(Q_X - Q_Y) / (Q_X + Q_Y)`. 