
ADCIRC
#############

.. figure:: images/models/adcirc_model.png
    :width: 500px
    :align: center
    :height: 250px
    :alt: alternate text
    :figclass: align-right

The ADvanced CIRCulation (ADCIRC; Schematic showing the preliminary mesh (left) and model results of flooding (right) near NSN in southwest Virginia from ADCIRC) model uses the continuous-Galerkin finite element method to solve modified forms of the shallow water equations on unstructured meshes (Luettich and Westerink, 2004; Westerink et al., 2008). Water levels are calculated using the generalized wave continuity equation, which is a combined and differentiated form of the continuity and momentum equations (Kinnmark, 1986). Depth-averaged current velocities are calculated from the vertically integrated momentum equations. ADCIRC has achieved prominence in storm surge forecasting (Blanton et al., 2012; Fleming et al., 2007), hindcasting (Bunya et al., 2010; Thomas et al., 2019), evaluation and design of protection systems (Ebersole et al., 2007), and development of flood risk maps (FEMA, 2021). ADCIRC has been coupled with the Simulating WAves Nearshore (SWAN) model (Booij et al., 1999; Zijlema, 2010) to yield SWAN+ADCIRC (Dietrich et al., 2011b, 2012). SWAN+ADCIRC has been used operationally to forecast waves and coastal flooding during recent hurricane seasons, with guidance posted online (CERA; 2020; https://cera.coastalrisk.live) and shared directly with managers (Rucker et al., 2021).

.. figure:: images/models/adcirc_flow.png
    :width: 400px
    :align: center
    :height: 250px
    :alt: alternate text
    :figclass: align-right

ADCIRC uses unstructured meshes with resolution ranging from kilometers in open water, to hundreds of meters near the coastline and through floodplains, and to tens of meters in the small- scale natural and man-made channels that convey surge into inland regions. Early project activities have included the development of meshes focusing on southwest Virginia, with mesh resolution down to 60 m near NSN. Pre-processing of the meshes included selecting digital elevation model (DEM) tiles with higher resolutions near Norfolk while reducing the resolution into the Atlantic Ocean. Using the DEMs, a shapefile was developed to represent the coastline and floodplains of the mesh to focus only on southwest Virginia. With the minimum resolution staged across the domains, most of the elements are centered at the NSN. A minimum resolution of 1 km was used across the Mid-Atlantic region and 10 km over the remainder of the Atlantic Ocean. OceanMesh2D (Roberts et al., 2019) was used to generate these finite element meshes.


`ADCIRC Documentation <https://http.StatusNotFound>`_