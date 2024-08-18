
EMPIRICAL MODEL DEMO
========================

.. figure:: images/demo/emp_norfolk.png
    :width: 400px
    :align: center
    :height: 225px
    :alt: alternate text
    :figclass: align-right

    Figure 1. An initial estimate from the empirical model.

**Empirical Model Norfolk**The Russo model (Russo, 1998) relates the surge, :math:`S`, to several storm and geometry factors  .. math::    S=PBC
where :math:`P` is a central pressure factor, :math:`B` is a bathymetry correction factor, and :math:`C` is a factor to account for hurricane speed and angle with respect to the shoreline. All three factors have additional empirical formulations embedded within. 
An example for Norfolk for Hurricane Irene is provided assuming the following information as obtained from local shoreline geometry and NOAA information prior to Irene. Estimates are: 
 * Lowest central pressure: 951 mbar
 * Forward speed: 20 km/hr
 * Approach angle: 240°
 * Latitude: 4097 km from equator
The “model” is run for a range of forward speeds and angles with the specific parameters for Norfolk also shown. This simple estimate is within roughly 0.5 m of the surge for Irene estimated by NOAA data. The same approach will be conducted for the other chosen events and more complete comparison will be made to available NOAA data and Class I-III models.For the runup models, model bathymetry for the Atlantic shoreline adjacent to Norfolk will be used to construct alongshore-spaced beach profiles. The foreshore slope will be identified from +/- 1 m of the mean waterline. Wave information will be extracted from the Delt3D simulation along the 10 m contour. Runup exceedance will be determined form these parameters. An initial estimate is shown in Figure 1. 