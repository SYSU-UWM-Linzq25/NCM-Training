## Lecture 2: The implementation of the reservoir management module built-in CREST
### Part 1 Rainfall-runoff generation
Keywords: `CREST-3.5` `Reservoir` `Modeling` 

<div align="center">
<br>Xinyi Shen, PhD<br>
<br>xinyis@uwm.edu<br>
</div>

### Scope
This section details the simulation of rainfall-runoff in CREST grids that are influenced by reservoirs, and describes the partitioned ET computation.

### Goals
* Simulate rainfall-runoff and partitioned ET in mixed land-reservoir grids
* Understand Penman-based ET and corrections
* Configure and run CREST with reservoir inputs

### Step-by-Step

#### 1. Model Structure (CREST 3.0 → 3.5)
- CREST-3.0 Model Structure
  - It is easier to use an incremental approach to understand the reservoir module
   <div align="center">
   <img src="images/Fig14.png" width="80%">
   </div>

- CREST-3.5 Model Structure Updates
  - For a grid that potentially that reservoir takes up f fraction (0<f<1) 
   <div align="center">
   <img src="images/Fig15.png" width="80%">
   </div> 
    - Reservoir fraction f can be read from the reservoir_fraction mask
    - The inundated status will be determined in the routing process
     - Reservoir DEM and the current stage will be used to evaluate if a reservoir grid is inundated

#### 2. Important Variables in the CREST model
  - The computation of E<sub>land
    - Penman-Monteith equation-energy
       <div align="left">
       <img src="images/Fig16.png" width="30%">
       </div> 

  - Conductance is the reciprocal of resistance
    - 𝑟<sub>0 </sub>− Canopy architecture resistance
    - 𝑟<sub>𝑐 </sub>− Soil Water deficit resistance 
    - 𝑟<sub>𝑤 </sub>−Atmospheric resistance

  - Maximal ET rate of overland areas
    - 𝑊<sub>𝑖 </sub>− intercepted water
    - 𝑊<sub>𝑖𝑚 </sub>− maximal interception capacity

    - Evaporation Rate in the Canopy layer
       <div align="left">
       <img src="images/Fig17.png" width="30%">
       </div> 
    
    - Transpiration rate on the ground
       <div align="left">
       <img src="images/Fig18.png" width="30%">
       </div> 

    - Evaporation rate on the ground
       <div align="left">
       <img src="images/Fig19.png" width="30%">
       </div> 

  - ET-Partition-Time Partition Approach
    - Time fraction of exhausting interception 
       <div align="left">
       <img src="images/Fig20.png" width="30%">
       </div> 

    - Time fraction of exhausting surface water with canopy evaporation
       <div align="left">
       <img src="images/Fig21.png" width="30%">
       </div> 

    - Time  fraction  of exhausting  the rest  surface water
       <div align="left">
       <img src="images/Fig22.png" width="30%">
       </div> 

  - AET rate
       <div align="left">
       <img src="images/Fig23.png" width="30%">
       </div> 

  - The computation of E<sub>lake 
    - Setting  canopy and soil resistance to 0, we have the Penman-Monteith equation recede to  Penman Equation
       <div align="left">
       <img src="images/Fig24.png" width="30%">
       </div>   
    - Penman Equation combines the energy balance and turbulence approaches and is the most widely used equation of Potential Evapotranspiration
    
  - Saturated Vapor Pressure solely depends on temperature
    - Saturated Vapor Pressure
       <div align="left">
       <img src="images/Fig25.png" width="20%">
       </div>    
    - An analytical approximation 
      - e is in (KPa)
      - T is in oC
       <div align="left">
       <img src="images/Fig26.png" width="30%">
       </div>   
    - Actual Vapor Pressure & Relative Humidity
       <div align="left">
       <img src="images/Fig27.png" width="30%">
       </div>  

  - Physics of Evaporation
    - Friction caused by surface roughness reduces V near the ground and produces turbulent eddies
    - The vertical components of the eddies provide the means for exchange of momentum, sensible heat and water vapor between the land surface and atmosphere
       <div align="left">
       <img src="images/Fig28.png" width="30%">
       </div>   

  - The Momentum Approach
    - Recall the mass-transfer equation
      - Latent Heat
        <div align="left">
        <img src="images/Fig29.png" width="30%">
        </div>   
        - K<sub>E is only weather related

          <div align="left">
          <img src="images/Fig30.png" width="30%">
          </div>   
          <div align="left">
          <img src="images/Fig31.png" width="30%">
          </div>   
        - aerodynamic resistance (red rectangle)
          <div align="left">
          <img src="images/Fig32.png" width="30%">
          </div>   

      - Assume weather is known, what is the only unknown to compute ET?
  
  - Sensible Heat
      <div align="left">
      <img src="images/Fig33.png" width="30%">
      </div>  

  - Bowen Ratio
      <div align="left">
      <img src="images/Fig34.png" width="30%">
      </div>  

  - ET Computation-The Energy Balance Approach
    - The energy balance for an evapotranspiration body:
      <div align="left">
      <img src="images/Fig35.png" width="30%">
      </div>  

    - In terms of Bowen ratio
      <div align="left">
      <img src="images/Fig36.png" width="30%">
      </div>  
      <div align="left">
      <img src="images/Fig37.png" width="30%">
      </div>  

  - The Combined Approach-Penman Equation (Penman 1948)
    - Penman Equation: Neglect G, Aw and dU/dt
      <div align="left">
      <img src="images/Fig38.png" width="20%">
      </div>     
    - The slope of saturation vapor pressure vs. T curve
      <div align="left">
      <img src="images/Fig39.png" width="30%">
      </div>   
      <div align="left">
      <img src="images/Fig40.png" width="30%">
      </div>     
    - Rewrite sensible heat using latent heat
      <div align="left">
      <img src="images/Fig41.png" width="30%">
      </div>   
    - Rearranging gives the Penman Equation:
       <div align="left">
      <img src="images/Fig42.png" width="30%">
      </div>   
    
      - Advantages?
      - Drawbacks?
  
 - Penman is inaccurate in lakes/reservoirs
    - Main reason
      - Heat storage of lake water is ignored
    - Main conflicts
      - Water depth is unknown before routing
      - It is difficult to model lake EB without knowing the depth first
      - Land surface runs before routing 
      - Calibration requires routing to be one-way coupled with the land surface module
    - Solution-empirical calibration
      - ET<sub>lake</sub>=ET<sub>penman </sub>x exp (-kD) 
      - ET<sub>lake</sub>=ET<sub>penman </sub>x aD<sup>b


#### 3. Interface of CREST v3.5
  <div align="center">
  <img src="images/Fig43.png" width="60%">
  </div>   
    
  - Let’s change the result and forcing directories for your own output
  <div align="center">
  <img src="images/Fig44.png" width="60%">
  </div>   

  - And please also copy the forcing control files to your forcing directory
  
#### 4. Hands on
  - Forecast the Flood event on Jan 6-25
  - Example control files are stored at 
  - /g/model/hydro/hydrowork/CREST_work/dummy_projects/MidWestCoast/Lzq25_New/Time_class
      ```
      run_m1.conf
      run_m1_new.project
      run_m1.sh
      ```

  - Edit the control file
    <div align="left">
    <img src="images/Fig45.png" width="30%">
    </div>   
    - Dam and Reservoir Section
      <div align="left">
      <img src="images/Fig46.png" width="30%">
      </div>   

  - Copy the observation folder to your own directory and modify the control file accordingly 
    <div align="left">
    <img src="images/Fig47.png" width="30%">
    </div>   

  - Modify the forcing folder to your own path
  - Copy the forcing control file to the your path
  - Set the control file accordingly
    <div align="left">
    <img src="images/Fig48.png" width="30%">
    </div>   

  - Do the same for the result folder
    <div align="left">
    <img src="images/Fig49.png" width="30%">
    </div>

  - $ vi run_m1.conf
    <div align="left">
    <img src="images/Fig50.png" width="30%">
    </div> 
  - point to your own control file and request for the correct number of resources
  
  - Do the same for the bash file
    -  $vi run_m1.sh
    -  Point to your own conf file and request the correct cluster and number of resources matching the configuration file
        <div align="left">
        <img src="images/Fig51.png" width="30%">
        </div>      

  - Submit the job
    -  $ sbatch run_m1.sh

  - Check the progress and job status
    -  $squeue – u your_account
    -  The output file is located in the red box after -o
        <div align="left">
        <img src="images/Fig52.png" width="30%">
        </div>    

  - Runs 3 steps in a role
  - Import forcing
  - Land surface simulation
  - Mosaic
    <div align="left">
    <img src="images/Fig53.png" width="30%">
    </div> 


  - Forecast the Flood event on Jan 6-25
    -  Set up the control file focusing on the added reservoir sections
    -  Visualize runoff, and E<sub>lake
       <div align="center">
       <img src="images/Fig54.png" width="60%">
       </div>  

       <div align="center">
       <img src="images/Fig55.png" width="30%">
       </div> 








