**Lecture 1: Concept of Reservoir Management and Data Preparation**

**Objective**
- Understand the concept of reservoir modules
- Learn how to delineate the maximal range of reservoirs
- Generate stage-storage curves and upscaled reservoir masks for CREST

**Key Concepts**
- Reservoir water balance
- Stage-storage curve generation
- Reservoir reconstruction from DEM
- Fraction and ID masks

**Step-by-Step**

**1. Understand Why a Reservoir Module is Needed**
- Saudi Arabia case: heavy regulation by reservoirs
- Reservoir level = only calibration truth in some regions

**2. Assumptions in the Model**
- Dam gates: bottom-opening
- Controllable outflow: gates/holes
- Uncontrollable: spillways

**3. Reservoir Water Balance Logic**
```
Total Inflow = Outflow (controlled + uncontrolled) + Evaporation + ΔStorage
```
- P already included in runoff generation

**4. Data Requirements**
- High-resolution DEM (e.g., 5m DTM)
- Medium-resolution hydrography (e.g., MERIT, HydroSHEDS)

**5. Maximal Reservoir Range Delineation**
- Delineate upstream using 90m MERIT
- Snap dam to stream, seed lowest pixel, fill upstream extent
- Guess reference elevation using downstream pixel

**6. Calibrate Reference Elevation**
- Tune elevation to match storage from NCM record
- Step 1: Coarse search (2m)
- Step 2: Fine-tune (0.2m)

**7. Stage-Storage Curve Generation**
- Loop from min stage to dam elevation
- Calculate:
  - Storage (volume)
  - Area
  - Stage (elevation)

**8. Generate Masks at 5m**
- DEM Mask
- Reservoir Mask (GeoTiff, NoData = 0)
- ID Mask (GeoTiff, runoff association)

**9. Upscale to 500m**
- Mosaic 5m masks
- Convert to 500m grids
- Set NoData: -9999 (DEM & ID), 0 (binary mask)

**10. Dam Location Correction**
- Multiple dams may fall on the same 500m grid
- Manually adjust dam locations to separate IDs

**Hands-on (See Lecture 1 Hands-on Slides)**
- Virtually mosaic 5m DTM
- Run delineation algorithm
- Generate 5m masks
- Upscale to 500m

**Summary**
- The foundation for reservoir modeling is understanding the geometry and capacity
- These pre-processing steps are critical before applying dynamic routing or reservoir operation logic

---

