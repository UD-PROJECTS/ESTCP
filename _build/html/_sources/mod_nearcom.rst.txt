
NearCoM 
######################

.. figure:: images/models/nearcom_model.png
    :width: 550px
    :align: center
    :height: 250px
    :alt: alternate text
    :figclass: align-right

The Nearshore Community Model (NearCoM) is an extensible, user-configurable model system for nearshore wave, circulation and sediment processes developed during the National Oceanographic Partnership Program (NOPP). The model consists of a “backbone”; the master program, handling data input and output as well as internal storage, together with a suite of modules, each of which handles a focused subset of the physical processes being studied. A total of 10 modules exist; developed by a large group of researchers from various institutions. Example modules are: 1) A wave module simulates wave transformation over arbitrary coastal bathymetry and predicts radiation stresses and wave-induced mass fluxes; 2) A circulation module simulates the slowly varying current field driven by waves, wind and buoyancyforcing, and provides information on the bottom boundary layer structure; and 3) A seabed module simulates sediment transport, determines the bedform geometry, parameterizes the bedform effect on bottom friction, and computes morphological evolution resulting from spatial variations in local sediment transport rates.

The figure above shows the computational grid (left) and an example of modeled surface elevation induced by an extreme storm event at Norfolk (right).

.. figure:: images/models/nearcom_flow.png
    :width: 450px
    :align: center
    :height: 250px
    :alt: alternate text
    :figclass: align-right

Recently, a new model coupling system called NearCoM-TVD (Total Variation Diminishing) was developed based on the MPI-based parallel computing framework. NearCoM-TVD couples a nearshore circulation model, SHORECIRC, using a hybrid finite-difference finite-volume TVD- type scheme on a generalized curvilinear grid, the wave model SWAN, and several selectable sediment transport modules (e.g. Kobayashi et al., 2008; Soulsby, 1997; Van Rijn et al., 2011) as shown in Figure 8. NearCoM-TVD is the standard version open to public and maintained in the GITHUB repository. Figure 7 shows the generalized curvilinear grid with the fine grid resolution
at NSN and an example of modeled surface elevation by an extreme storm event at Norfolk. NearCoM-TVD is an open source code maintained in GITHUB with report (Chen et al., 2014; Shi et al., 2013) and online (WIKI page: https:// fengyanshi.github.io /NEARCOM-TVD /WIKI/ _build/ html/index.html) documentation.

`NearCoM Documentation <https://fengyanshi.github.io/NEARCOM-TVD/WIKI/_build/html/index.html>`_

`NearCoM Github Repository <https://github.com/fengyanshi/NEARCOM-TVD>`_

