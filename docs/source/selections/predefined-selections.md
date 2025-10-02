# Pre-defined selections

The following selections are available in this library, divided in separate namespaces according to which subdetector they focus on.

## Trigger

*Under namespace* `NSL::Selections::Trigger`

| Name              | Description                                      | Parameters |
|-------------------|--------------------------------------------------|------------|
| HasPhysicsTrigger | Check if the event is triggered by a physics trigger | None       |

### HasPhysicsTrigger

This selection simply returns the value of `NAIA::EventSummary::IsPhysicsTrigger()`

## Tof

*Under namespace* `NSL::Selections::Tof`

| Name             | Description                                           | Parameters                                  |
|------------------|-------------------------------------------------------|---------------------------------------------|
| BetaInRange      | Check if the Tof beta measurement is in a given range | Range lower bound, Range upper bound, Beta measurement type |
| ChargeInRange    | Check if the Tof charge measurement is in a given range | Range lower bound, Range upper bound, Charge measurement type |
| GoodPathlength   | Check if a given combination of Tof layers have a well defined track pathlength | A 4-bit bitset encoding the layer pattern to be checked |
| Chi2CooLessThan  | Check if the spatial chi-square of the ToF clusters is less than given value | Upper bound |
| Chi2TimeLessThan | Check if the temporal chi-square of the ToF clusters is less than given value | Upper bound |

### BetaInRange

This selection checks if the Tof beta is within a specified range. You can specify the beta range, as well as the `BetaType` to check. 

### ChargeInRange

This selection checks if the Tof charge is within a specified range. You can specify the charge range, as well as the `ChargeType` to check. 

### GoodPathlength

This selection checks if the Tof layers have a well defined pathlength. This means that either the Tracker track (or the Tof linear interpolation based
on the clusters position) crosses the Tof paddles from top to bottom (and doesn't intercept the sides). You can specify which layers should be checked by
passing a 4-bit bitset when creating the selection.

The 4 bits are associated with Tof layers in order from least significant to most significant (or right-to-left, if you will), meaning that if you want to 
check only Upper Tof layers the corresponding value is `0b0011`, while for Lower Tof is `0b1100` (and `0b1111` for all 4 layers).

### Chi2CooLessThan

This selection checks if the normalized chi-square of the Tof clusters spatial reconstruction is less than given value. You can specify the upper bound.

### Chi2TimeLessThan

This selection checks if the normalized chi-square of the Tof clusters temporal reconstruction is less than given value. You can specify the upper bound.

## Tof Standalone

*Under namespace* `NSL::Selections::TofSt`

| Name             | Description                                           | Parameters                                  |
|------------------|-------------------------------------------------------|---------------------------------------------|
| BetaInRange      | Check if the Tof beta measurement is in a given range | Range lower bound, Range upper bound, Beta measurement type |
| ChargeInRange    | Check if the Tof charge measurement is in a given range | Range lower bound, Range upper bound, Charge measurement type |
| Chi2CooLessThan  | Check if the spatial chi-square of the ToF clusters is less than given value | Upper bound |
| Chi2TimeLessThan | Check if the temporal chi-square of the ToF clusters is less than given value | Upper bound |
| InnerFiducialVolume | Check if the Tof track lies within a pre-defined fiducial volume defined on the individual tracker planes of the Inner Tracker | None |
| L1FiducialVolume    | Check if the Tof track lies within a pre-defined fiducial volume defined on the L1 tracker plane. | None |
| L9FiducialVolume    | Check if the Tof track lies within a pre-defined fiducial volume defined on the L9 tracker plane. | None |

The `standalone` selections for Tof are constructed from the particle 0 Tof objects, but this reconstruction avoids using any information from the Tracker track. It's meant to be used for the Track reconstruction efficiency evaluation.
The first four cuts coincide with the non standalone version, whose description is given in the `Tof` section.
Note that the `ChargeInRange` cut uses `NAIA::TofBaseData::ChargeNoPL`, instead of `NAIA::TofBaseData::Charge`, due to an unresolved bug with the latter in the current version of NAIA (1.2).
The fiducial volume cuts are meant to ensure that the event lies within the tracker volume, using only the linear track given by Tof clusters. See the namesake selections in `Full Track` section for their description.

## Trd Standalone

*Under namespace* `NSL::Selections::TrdSt`

| Name                | Description                                           | Parameters                                  |
|---------------------|-------------------------------------------------------|---------------------------------------------|
| InnerFiducialVolume | Check if the Trd track lies within a pre-defined fiducial volume defined on the individual tracker planes of the Inner Tracker | None |
| L1FiducialVolume    | Check if the Trd track lies within a pre-defined fiducial volume defined on the L1 tracker plane. | None |
| L9FiducialVolume    | Check if the Trd track lies within a pre-defined fiducial volume defined on the L9 tracker plane. | None |

The `standalone` selections for Trd are constructed using the Tof extrapolation to select TRD hits, without using any information from the Tracker track. Meant to be used for the Track reconstruction efficiency evaluation.
The fiducial volume cuts are meant to ensure that the event lies within the tracker volume, using only the linear track given by Trd hits. See the namesake selections in `Full Track` section for their description.

## InnerTracker

*Under namespace* `NSL::Selections::InnerTracker`

| Name                      | Description                                                                 | Parameters                                  |
|---------------------------|-----------------------------------------------------------------------------|---------------------------------------------|
| ChargeInRange             | Check if the Inner Tracker charge measurement is in a given range           | Range lower bound, Range upper bound, Charge measurement type |
| ChargeRMSLessThan         | Check if the Inner Tracker charge RMS is less than a given value            | Upper bound, Charge measurement type        |
| HitPattern                | Check if the track hits satisfy the canonical pattern* | None                                        |
| NHitsGreaterThan          | Check if the track has at least a minimum number of hits on a given side    | Lower bound, Tracker side                   |
| NGoodClustersGreaterThan  | Check if the track has at least a minimum number of hits with a good charge status | Lower bound, Cluster status mask         |

### ChargeInRange

This selection checks if the Inner Tracker charge is within a specified range. You can specify the charge range, as well as the `ChargeRecoType` to check. 

### ChargeRMSLessThan

This selection checks if the Inner Tracker charge RMS is lower than a specified value. You can specify the upper limit, as well as the `ChargeRecoType` to check. 

### HitPattern

This selection checks if the Inner Tracker hits satisfy the canonical `L2 && (L3 || L4) && (L5 || L6) && (L7 || L8)` pattern. 

### NHitsGreaterThan

This selection checks if the Inner Tracker track has at least a minimum number of hits on a given side. You can specify the lower limit of hits, as well as the Tracker side to consider.

### NGoodClustersGreaterThan

This selection checks if the Inner Tracker track has at least a minimum number of clusters with a good status on a given side. You can specify the lower limit of hits, as well as the mask to compare the hit status against (see [here](https://ams.cern.ch/AMS/Analysis/hpl3itp1/root02_v5/html/development/html/classTrClusterR.html#a24ef522472bd83d45174daee1f1853a9) for details on the charge status word).


## Full Track

*Under namespace* `NSL::Selections::Track`

### Track-related selections

| Name                      | Description                                                                 | Parameters                                  |
|---------------------------|-----------------------------------------------------------------------------|---------------------------------------------|
| ChiSquareLessThan         | Check if the Track chi-square is less than a specified threshold, for a given side, fit and span. | Upper bound, tracked side, track fit algorithm, track span. |
| InnerFiducialVolume       | Check if the Track lies within a pre-defined fiducial volume defined on the individual tracker planes of the Inner Tracker. | Track fit algorithm, track span. |
| L1FiducialVolume          | Check if the Track lies within a pre-defined fiducial volume defined on the L1 tracker plane. | Track fit algorithm, track span. |
| L9FiducialVolume          | Check if the Track lies within a pre-defined fiducial volume defined on the L9 tracker plane. | Track fit algorithm, track span. |
| HitCut                    | Check if the Track has a hit on a given tracker layer. | Layer J-number (1...9). |
| L1NormResidualLessThan    | Check if the normalized residual on L1 is below a specified threshold, for a given fit. | Upper bound, track fit algorithm. |

### ChiSquareLessThan

This selection checks if the track has chi-square per degree of freedom less than a specified threshold, on a given side. You need to specify a track fit algorithm and a track span.

### InnerFiducialVolume

This selection checks if the track lies inside a pre-defined fiducial volume within the Inner Tracker. This fiducial volume is defined removing the most external part of the inner tracker layers. You need to specify a track fit algorithm and a track span.

#### Fiducial volume definition

| Layer | R boundary | Y boundary |
|-------|------------|------------|
| L2    | 62 cm      | 40 cm      |
| L3 / L4 | 46 cm     | 44 cm      |
| L5 / L6 | 62 cm     | 36 cm      |
| L7 / L8 | 46 cm     | 44 cm      |

### L1FiducialVolume

This selection checks if the track lies inside a pre-defined fiducial volume within the L1 plane. This fiducial volume is defined removing the most external part of the L1 plane. You need to specify a track fit algorithm and a track span.

#### Fiducial volume definition

| Layer | R boundary | Y boundary |
|-------|------------|------------|
| L1    | 62 cm      | 47 cm      |

### L9FiducialVolume

This selection checks if the track lies inside a pre-defined fiducial volume within the L9 plane. This fiducial volume is defined removing the most external part of the L1 plane. You need to specify a track fit algorithm and a track span.

#### Fiducial volume definition

| Layer | R boundary | Y boundary |
|-------|------------|------------|
| L9    | 43 cm      | 29 cm      |

### HitCut

This selection checks if the Track has a hit on a given tracker layer (effectively equivalent to check `TrTrackR::GetHitLJ` in gbatch).

### L1NormResidualLessThan

Check if the normalized residual on L1, defined as `\(\chi^2_{IL1} \cdot NDoF_{IL1} - \chi^2_{Inner} \cdot NDoF_{Inner}\)`, is below a specified threshold, for a given fit.

### L1NormResidualLessThan

Check if the normalized residual on L1, defined as `\(\chi^2_{IL1} \cdot NDoF_{IL1} - \chi^2_{Inner} \cdot NDoF_{Inner}\)`, is below a specified threshold, for a given fit.

## Tracker Layer Charges

*Under namespace* `NSL::Selections::TrackerLayer`

| Name              | Description                                                                                   | Parameters                                         |
|-------------------|-----------------------------------------------------------------------------------------------|----------------------------------------------------|
| ChargeInRange     | Check if the Tracker charge (combination of X and Y measurements) of a particular layer is in a given range | Layer J-number (1...9), Range lower bound, Range upper bound, Charge measurement type |
| ChargeStatus      | Check if the Track cluster on a particular layer has a good status (checked using the default `0x10013D` mask) | Layer J-number (1...9)                             |
| ChargeAsymmetry   | Check if the relative difference between Tracker charge on X and Y views for a particular layer is below a specified threshold | Layer J-number (1...9), Upper bound, Charge measurement type |

### LayerChargeInRange

This selection checks if the Tracker charge of a particular layer is in a given range. This check is based on the combined charge estimation from both X and Y sides. You can specify the charge range, as well as the `ChargeRecoType` to check.

### LayerChargeStatus

This selection checks if the Tracker charge of a particular layer has a good status word. [See here](https://ams.cern.ch/AMS/Analysis/hpl3itp1/root02_v5/html/development/html/classTrClusterR.html#a24ef522472bd83d45174daee1f1853a9) for details on the charge status word.

### LayerChargeAsymmetry

This selection checks if the Tracker charge asymmetry for a particular layer is below a specified threshold. The asymmetry is defined as \((Q_X - Q_Y) / (Q_X + Q_Y)\).

## Unbiased External Layer

*Under namespace* `NSL::Selections::UnbExtLayer`

| Name                      | Description                                                                 | Parameters                                  |
|---------------------------|-----------------------------------------------------------------------------|---------------------------------------------|
| UnbExtLayerHitCut  | Checks if there is a hit on either L1 or L9 | Layer J-number (1 or 9) |
| UnbExtLayerChargeInRange  | Checks if the unbiased charge of external layers (L1 and L9) is within a given range | Layer J-number (1 or 9), Range lower bound, Range upper bound |

### UnbExtLayerHitCut

This selection checks the presence of a hit on external layers (L1 or L9). It is meant to constrain the denominator selection for the Inner Tracker efficiency.

### UnbExtLayerChargeInRange

This selection checks if the unbiased charge of external layers (L1 or L9) is within a given range. It is meant to constrain the denominator selection for the Inner Tracker efficiency.

## Event Summary

*Under namespace* `NSL::Selections::EvSummary`

| Name                | Description                                                       | Parameters  |
|---------------------|-------------------------------------------------------------------|-------------|
| NAccLessThan        | Check if number of fired Acc clusters are less than given value   | Upper bound |
| NTofClusterLessThan | Check if number of hit Tof clusters are less than given value     | Upper bound |
| NTrTrackLessThan    | Check if number of reconstructed tracks are less than given value | Upper bound |

These selections use the `Event Summary` informations to require a stricter sample for efficiencies calculation. They are meant to correct the time dependence of Layer 1 efficiency, ensuring a clean selection of events for its denominator.

### NAccLessThan

This selection checks if the number of fired Acc clusters is less than a given value. You can specify the upper bound.

### NTofClusterLessThan

This selection checks if the number of hit Tof clusters is less than a given value. You can specify the upper bound.

### NAccLessThan

This selection checks if the number of reconstructed tracks is less than a given value. You can specify the upper bound.

## RTI

*Under namespace* `NSL::Selections`

| Name              | Description                                      | Parameters |
|-------------------|--------------------------------------------------|------------|
| DefaultRTISelection | Default RTI selection as in Pass8 Twiki | NAIA::RTIInfo object, is_photon_polarization_run boolean |
| PG_ProtonRTISelection | PG Proton Analysis RTI selection | NAIA::RTIInfo object, UTC time in seconds |

### DefaultRTISelection

Standard collection of cuts on RTI, used in many analyses. It's derived from the AMS Pass8 Twiki.

### PG_ProtonRTISelection

This selection is used in the PG Proton Analysis to increase statistics at low rigidity (mainly for SEP events).

## Auxiliary Cuts

*Under namespace* `NSL::Selections::Aux`

| Name              | Description                                      | Parameters |
|-------------------|--------------------------------------------------|------------|
| MassCutProton | Cuts charge 1 light particles (pions and kaons) based on 1/Beta | track fit algorithm, track span, Beta measurement type|

### MassCutProton

This selection is derived from the AMS Twiki and is used to clean the proton sample from light particles produced by interactions (pions and kaons). It can be applied on both ISS data and Monte Carlo to clean the selected sample and the denominator of the L1 efficiency.

# Common Selections

There is a set of selections recommended by the MIT group for nuclei analysis, and they are commonly referred to as "common selections". These selections are a special case of the predefined selections listed above in this page, but where the selection parameters are tuned with respect to the particle charge under exam.

These specialized versions of predefined selections can be found under the `NSL::Selections::Common` namespace, with the same exact class name as their base version.

# PG-ProtonAnalysisSelection

The global header `NSL/PG-ProtonAnalysisSelection.h` includes the current proton selection used in Perugia for the Daily and SEP analyses, using NSL cuts. It contains the primary selection for counts (`selection`) and all the numerators and denominators of efficiencies (`den_l1` and `num_l1` for Layer 1 eff, `den_tf` and `num_tf` for the ToF eff...).