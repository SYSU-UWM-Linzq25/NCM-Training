**Lecture 3: Reservoir Routing and Scenario Simulation**

**Objective**
- Understand how routing interacts with reservoirs in CREST
- Learn how to set up routing paths and control structures
- Forecast discharge and storage under general vs. emergency conditions

**Key Concepts**
- Routing path modification and dam snapping
- Confluence behavior and dam-controlled flow
- Regulation plans and operational rules
- Scenario-based flood analysis

**Step-by-Step**
- Snap dam to stream grid (downstream-most in reservoir area)
- Redirect upstream cells to flow through dam cell
- Bypass parallel branches if needed
- Define regulation plans (e.g. 4 seasonal stages)
- Assign dam emergency status (text file + control file)
- Run routing using 1 CPU: `./CREST_RT run_m1.project`
- Analyze storage and discharge in 4 flood events

**Summary**
- CREST routing integrates physical and policy-driven controls
- Dam snapping and ID masking are essential
- Scenario results highlight the importance of dynamic regulation
