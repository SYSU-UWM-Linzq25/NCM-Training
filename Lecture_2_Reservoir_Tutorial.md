**Lecture 2: Rainfall-Runoff and ET Estimation in CREST**

**Objective**
- Understand how the CREST model simulates rainfall-runoff in grids containing reservoirs
- Learn the evapotranspiration (ET) processes and how they differ over land and reservoir areas
- Set up control files and submit a simulation job on HPC

**Key Concepts**
- Reservoir fraction (f)
- AET (actual ET) partitioning
- Penman and Penman-Monteith methods
- CREST routing interface and job structure

**Step-by-Step**

**1. Model Structure Overview**
- Start with CREST 3.0: rainfall-runoff coupling
- Update to CREST 3.5: reservoir fraction (f) added
- Each 500m grid may be partially covered by reservoir (0 < f < 1)

**2. Partitioning ET Between Land and Reservoir**
```
AET = (1 - f) * ET_land + f * ET_lake
```
- f is read from the reservoir_fraction mask
- Inundation status determined during routing
- Over reservoir: force saturated soil, no transpiration

**3. Evaporation from Reservoirs (ET_lake)**
- Simplified Penman equation:
  - Set canopy/soil resistance = 0
  - Use temperature, radiation, humidity, wind
- Limitations:
  - Penman overestimates in lakes (ignores heat storage)
  - CREST runs land module before routing

**4. Empirical Correction of ET_lake**
```
ET_lake = ET_penman * exp(-kD)   or   ET_lake = ET_penman * aD^b
```
- D = water depth
- Calibrated via empirical parameters (a, b or k)

**5. Preparing the Simulation**
- Modify result and forcing directories
- Update forcing control file paths
- Copy observation folder
- Update reservoir section

**Example:**
```bash
vi run_m1.conf   # edit config file
vi run_m1.sh     # edit SLURM script
sbatch run_m1.sh # submit job
```

**6. Run Simulation**
- Import forcing
- Land surface simulation
- Mosaic
- Visualize E_lake and runoff using output files

**Summary**
- ET partitioning is key to reservoir-land coupling
- Penman-based ET estimation can be calibrated for reservoir depth
- CREST’s modular structure allows customizing reservoir processes via control files

---

