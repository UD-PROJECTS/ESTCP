
Overview of Technology/Methodology
###########################################

.. figure:: images/models/emperical_model.png
    :width: 400px
    :align: center
    :height: 175px
    :alt: alternate text
    :figclass: align-right

Model Classes
******************

TWLs will be predicted using a suite of simulation models from simple, low-cost empirical to complex, high cost (computation and effort) physics-based. Table 1 provides the proposed list and technical aspects of the models. The modeling approaches can be grouped into three classes. Class I includes empirical approaches that focus on simplified equations for storm surge (Russo, 1998) and runup (Stockdon, et al., 2006). Class II includes (coupled) process-based numerical models such as ADCIRC (Dietrich et al., 2011b; Luettich and Westerink, 2004), NearCoM (Shi et al., 2013) and Delft3D (Lesser et al., 2004). The models are essentially based on the Nonlinear Shallow Water Equations (NSWE) and resolve tides, surges, and statistical wave conditions. The models are dynamically coupled to spectral wave models such as SWAN (e.g. Sebastian et al., 2014). These models (except ADCIRC) have morphodynamics modules to compute sediment transport and bed level changes. Class III includes dynamical wave models that resolve the waves. The model used here is called FUNWAVE-TVD (Shi et al., 2012). The model is computationally demanding and can include morphological change. Class III models predict the effects of incident sea and swell (SS), IG, and very low frequency (VLF) waves on TWL (Gawehn et al., 2016); processes largely ignored in past efforts.

.. figure:: images/models/overview.png
    :width: 600px
    :align: center
    :height: 800px
    :alt: alternate text
    :figclass: align-center

Advantages and Limitations of the Technology/Methonology 
***********************************************************

The demonstration seeks to determine suitability of a variety of prediction tools and identify ability to reach certain performance metrics. Of course, there are trade-offs depending on the numerical model used, the accuracy and output sought, the computational need, and manpower cost (Table 1); the latter related to expertise and experience with a particular modeling approach. The empirical models (Class I) are the most simple and cost-effective, but also provide the least fidelity. The different Class II models used are similar in their output and cost. The three models have been validated under a range of scenarios by the scientific and engineering community. However, the models lack the ability to resolve waves. The lone Class III model tested in this demonstration resolves waves, has a high spatial and temporal resolution, and a high computational cost. It cannot resolve tide and surge and requires Class II models to provide nearshore boundary conditions for model forcing.There are numerous other numerical models used for hydrodynamic predictions; and only a subset can be realistically tested in this demonstration. For example, the Class II models (Delft3D, ADCIRC) share similar strengths and weaknesses to other models in this class. For example, one similar model is SLOSH which is used extensively by the U.S. National Weather Service (NWS) to estimate storm surge heights resulting from hurricanes (Jelesnianski et al., 1992). SLOSH solves simplified forms of the shallow water equations on a polar, elliptic, or hyperbolic grid, telescopic outward with a finer resolution near the center (coastline). SLOSH storm surge predictions depend strongly on accurate meteorological input such as hurricane size, intensity, forward speed, trajectory, and atmospheric pressure (Forbes et al., 2014). SLOSH-based models are computationally efficient and are used for ensembles of predictions in real-time and for
climatological surge studies (Glahn et al., 2009; Zachry et al., 2015). SLOSH’s strength is its efficiency; a SLOSH simulation can be carried out in a few minutes on a single computational core once bathymetry and forcing conditions are available. Thus, SLOSH can be applied for ensemble simulations to account for uncertainties in storm predictions. However, SLOSH’s weaknesses include a relatively coarse model resolution that limits the accuracy in any single simulation, and it has only recently been extended to include tides and wave coupling.Similarly, for Class III models, FUNWAVE is comparable to tools like XBeach Nonhydrostatic (XB-NH; Smit et al., 2010) and SWASH (Zijlema et al., 2011). XB-NH computes the depth- averaged flow due to waves and currents using the nonlinear shallow water equations with a nonhydrostatic pressure correction. It is often used to simulate nearshore wave propagation and runup along reef-lined coasts (Quataert et al., 2020). SWASH in one-layer mode uses the same formulations as XB-NH. However, SWASH can resolve variations in flow over the water depth by adding multiple vertical layers. As a result, SWASH is often used to simulate wave-structure interaction, including wave runup and overtopping, where variations in flow with depth can play a significant role (Suzuki et al., 2017).

