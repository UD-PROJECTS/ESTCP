
Empirical model
#################

.. figure:: images/models/sketch.png
    :width: 400px
    :align: center
    :height: 150px
    :alt: alternate text
    :figclass: align-right

Empirical models for components of TWL are largely data-driven or based on machine learning (e.g. Pearson et al., 2017; Suanez et al., 2016; Tadesse et al., 2020). The empiricism is generally manifested through two components: storm surge and runup. Storm surge is the excess water level due to the wind stress acting over the ocean surface. However, surge magnitudes will also vary based on atmospheric pressure, forward storm speed, angle of shoreline approach, radius of maximum winds, coastal topography/bathymetry, and funneling into narrow water bodies (e.g. Irish and Resio, 2010; Needham and Keim, 2014; Resio et al., 2009). Simplistic approaches relate the surge, S, as (see Russo, 1998).. math:: 𝑆 = 𝑃𝐵𝐶
where P is a central pressure factor, B is a bathymetry correction factor, and C is a factor to account for hurricane speed and angle with respect to the shoreline. All three factors have additional empirical formulations embedded within. Other relations generate surge with respect to some factor multiplied by the square of the wind speed divided by depth (e.g. WMO, 2011) or develop surge hydrographs as a function of time related to the peak surge elevation at landfall, storm duration, radius of maximum wind and forward speed (e.g. Xu and Huang, 2014). Only the Russo (1998) empirical approach is used for surge in this demonstration.Runup, R, is the summation of wave set up and swash motions. Most empirical relations for runup incorporate the Iribarren Number, 𝜉, (Iribarren and Nogales, 1949) sometimes referred to as the surf similarity parameter that relates the beach slope to the offshore wave steepness. Hunt (1959) suggested the normalized runup (by wave height) is a function of :math:`\xi` as.. math:: \frac{R}{H} = K \xi = K \frac{\beta}{\sqrt{H/L}}where :math:`H` is wave height, :math:`K` is a constant, :math:`\beta` is the beach slope, and :math:`L` is the wave length. The majority of subsequent empirical relations for runup or the 2% runup exceedance, R2%, use the Hunt formula as the root form (e.g. Holman, 1986; Park and Cox, 2016; Stockdon, et al., 2006). It is noted that until recently (Park and Cox, 2016) the Hunt type formulations did not explicitly include beach profile geometry other than beach slope. The Stockdon et al. (2006) equation for runup exceedance is
 .. math:: R_{2\%} = 1.1 \left[ 0.35 \beta (HL)^{0.5} +\frac{(HL(0.563\beta^2+0.004))^{0.5}}{2} \right]
where the wave height and wave length are the deep water values. The equation can be simplified greatly for extremely dissipative conditions (NOT the case at NSN) to.. math:: R_{2\%} = 0.73\beta(HL)^{0.5}


