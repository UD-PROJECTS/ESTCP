Tyndall
##########################


Prediction Approaches
=========================================
.. figure:: images/demo/tyndall_table1.png
    :width: 500px
    :align: center
    :alt: alternate text
    :figclass: align-right

Table 1. Pros and cons of the models used in the demonstration.

TWLs were predicted using a suite of simulation models from simple one-dimensional (1D) models to complex two-dimensional (2D), high-cost (computation and effort) physics-based models (Table 1; see Appendix A for extensive model setup). The approaches were grouped into two classes. Class II includes process-based numerical models, Delft3D FM (Deltares, 2025), ADCIRC (Dietrich et al., 2011b; Luettich and Westerink, 2004), and NearCom (Shi et al., 2013). The models are essentially based on the Nonlinear Shallow Water Equations (NSWE) and resolve tides, surges, and statistical wave conditions. The models are dynamically coupled to spectral wave models such as SWAN (e.g. Sebastian et al., 2014). 
Class III also includes the process-based numerical model, XBeach (Roelvink et al., 2009). XBeach was used in the surfbeat mode (XBeach-SB) for this demonstration. The surfbeat mode solves the shortwave amplitude separately from the long waves, currents, and morphological change, saving computational time by not simulating the shortwave phase. XBeach-SB predicts the effects of infragravity (IG) waves, unsteady currents, and swash on TWL. Finally, the process-based model CSHORE (Figlus et al., 2011; Kobayashi et al., 2008; Kobayashi and Zhu, 2022) is used in 1D cross-shore mode with 50 1-km-spaced adjacent nearshore transects including computed bathymetry changes.



Simulation Approaches
=========================================
.. figure:: images/demo/tyndall_table2.png
    :width: 500px
    :align: center
    :alt: alternate text
    :figclass: align-right

The demonstration is carried out in three phases. 
Phase 1: Numerous simulations were conducted using Class II models for Michael. These simulations are referred to as baseline simulations for comparison to subsequent simulations and serve as the output for model calibration/validation. The baseline simulations were used to investigate the effect of using the parametrized (Holland Model; hereafter as HM) (Holland, 2008, 1980; Holland et al., 2010) and modeled (ERA5) (Hersbach, 2023) wind forcing on the accuracy of prediction of the TWL and associated flooding at TAFB and MB, where Michael made landfall. The output was calibrated against measured water levels at different locations along the Gulf of America Coast.
Phase 2: Michael forcing was perturbed to estimate the impacts of changing the meteorological forcing (central pressure drop and the radius of maximum wind) on the model results. The impacts of environmental variability in terms of mean sea level and varying wind speeds were evaluated. In addition, a series of degradation scenarios was conducted to evaluate model performance when there is a deficit of accurate data. These degradation scenarios included bathymetry error, model grid resolution, and potential error in the hurricane track (Table 2). A one-to-one comparison was carried out to evaluate the ability of the different models to predict the TWL and its associated impacts on TAFB and MB.
Phase 3: Class III (CShore) and (XBeach) models were used to evaluate the beach profile and morphological changes associated with Michael forcing. The class III models were also used to resolve the coastal process that cannot be resolved using the class II models (wave-induced runup) and how they affect the flooding at TAFB and MB.


Performance objectives
==========================
.. figure:: images/demo/tyndall_table3.png
    :width: 500px
    :align: center
    :alt: alternate text
    :figclass: align-right

Table 3. Performance objective for the TAFB demonstration.

This demonstration tests a variety of performance objectives related to TWL-induced flooding at TAFB and adjacent beaches (Table 3).  
Objective 1: Document the time required to develop the model bathymetry from DEMs and the forcing boundary conditions. 
Objective 2: Document the run time and computational architecture used to conduct the various simulations. 
Objective 3: Quantify the model skill in predicting the timing of peak surge using available water level station data. 
Objective 4: Quantify the model skill in predicting the magnitude of peak surge using available water level station data. 
Objective 5: Quantify the model skill in predicting the duration of a particular flooding level using available water level station data. 
Objective 6: Quantify the flooded spatial area as a function of time and flooded depth and compare the results to available data.
Objective 7: Compare the Class III full physics models (XBeach and CSHORE) with the Class II models (Delft3D FM, ADCIRC, and NearCom) for the open coast portion of the study to determine the importance of the wave component to TWL. There is no performance metric for this comparison.
Objective 8: Conduct “degradation simulations” to the base model simulation to quantify prediction error when there is a deficit of information (resolution and/or accuracy). There is no performance metric for this comparison. However, these simulations provide critical information on prediction confidence when input and forcing data are imperfect (always the case in a predictive scenario). 
Objective 9: Model simulation results to be provided as layers in a webpage for program manager review. 


Presentation of Results
============================

The reulsts can be found in the report of demonstration and `User-interactive maps <map_tyndall.html>`_

Executive Summary
==========================
Extreme weather events, enhanced via climate change, are expected to threaten coastal zones including those containing military installations, with severe flooding and erosion. This demonstration was carried out to enhance the resilience and readiness of coastal military facilities by evaluating the combined impacts of hurricanes and climate change on the Total Water Level (TWL) and associated coastal flooding at and near Naval Station Norfolk (NSN) on the US east coast.
Numerous methods were employed for the TWL prediction including empirical models, hydrodynamic models (D-Flow FM, ADCIRC, and NearCom), an energy-based wave model (SWAN), and class II  models (XBeach, CSHORE). Two wind forces were used, a modeled wind force (ERA5) and a parametric wind force (Holland Model: HM). The models were used to predict the peak surge characteristics (magnitude, timing, and duration) and flood area characteristics (extent, and average and maximum flood depth) at TAFB during the historical hurricane (Michael). 
Sensitivity analysis was carried out for the climate change impacts (sea level rise (SLR) and wind speed (WS)), hurricane characteristics (central pressure drop (PD) and radius of maximum wind (RMW)), and potential inaccuracies in the model inputs (storm track (ST) error, bathymetry accuracy, and mesh resolution). Finally, the time required for model building and simulation runtime were recorded as a reference for future users. 
The models were calibrated against measurements along the US east coast where a good agreement between the measured and simulated water levels was obtained. TO BE CONTINUED


