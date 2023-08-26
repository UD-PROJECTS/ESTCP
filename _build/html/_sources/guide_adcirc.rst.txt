
ADCIRC USERS GUIDE
#####################################

The ADvanced CIRCulation model (ADCIRC,  `Luettich et al. (1992) <https://adcirc.org/wp-content/uploads/sites/2255/2018/11/1992_Luettich02.pdf>`_ and `Westerink et al. (2008) <https://coast.nd.edu/reports_papers/2008-MWR-wlfadrpdkp.pdf>`_)  couples the Simulating WAves Nearshore (SWAN) model (Booij et al., 1999; Zijlema, 2010) to yield SWAN+ADCIRC (Dietrich et al., 2011b, 2012).

`ADCIRC <https://adcirc.org/home/documentation/users-manual-v53/>`_ is a coastal ocean circulation model to predict tides, wind-driven setup and storm surge, riverine flows, and density-driven circulation and transport. ADCIRC uses the continuous-Galerkin finite-element method to solve modified versions of the shallow water equations on unstructured meshes. Its horizontal resolution can be varied from the open ocean (e.g. with typical mesh spacings of tens of kilometers), to the coast and overland, to small natural and man-made channels (e.g. tens of meters) that convey flows to inland regions. During storms, ADCIRC is used by forecasters at NOAA and in academic settings to predict storm surge and coastal flooding for decision support. Between storms, ADCIRC is used by hindcasters to  support disaster resilience, e.g. design of protection systems or the next-generation flood insurance rate maps. Co-PI Dietrich is a leading developer of ADCIRC, and his research team has improved its efficiency, enhanced its predictions of surface transport, and extended its capabilities for decision support.

.. figure:: images/guide/adcirc/HSOFS.png
    :width: 400px
    :align: center
    :height: 200px
    :alt: alternate text
    :figclass: align-right


Example of unstructured mesh to represent southeast Virginia, including the Naval Station Norfolk (NSN) near the center of the figure. Colors show the ground surface elevation (m MSL), and the triangular element sizes range down to about 500 m. The project team is developing meshes with higher resolution near NSN.


The video below shows Hurricane Florence's effects in coastal North Carolina. Vectors show the wind velocities (m/s), while the contours show the ADCIRC water levels (m NAVD88). Note the high waters at the open coast just north of Florence's landfall, in the western Pamlico Sound and up the estuaries of the Neuse and Tar-Pamlico rivers, and over the low-lying farmlands of Carteret County.

.. raw:: html

   <iframe width="560" height="315" src="https://www.youtube.com/embed/ol2nJ72wtMQ?rel=0" frameborder="0" allowfullscreen></iframe>

`ADCIRC Online Users Manual <https://adcirc.org/home/documentation/users-manual-v53/>`_

########################

Simulating WAves Nearshore (`SWAN <https://swanmodel.sourceforge.io/online_doc/swanuse/swanuse.html>`_, `Booij et al., 1999 <https://agupubs-onlinelibrary-wiley-com.udel.idm.oclc.org/doi/pdfdirect/10.1029/98JC02622>`_ and `Zijlema, 2010 <https://doi.org/10.1016/j.coastaleng.2009.10.011>`_) is a spectral wave model to predict the generation, evolution, and dissipation of ocean surface waves. SWAN solves the action balance equation for the deep water processes of wave generation, dissipation and the quadruplet wave-wave interactions, and supplements for shallow water processes of dissipation due to bottom friction, triad wave-wave interactions, and depth-induced breaking. SWAN was developed to use the finite-difference on regular grids in the nearshore, but it was extended to run on the same unstructured meshes as ADCIRC. When coupled tightly as ADCIRC+SWAN, the models alternate their time-stepping, pass information through local memory without interpolation, and improve predictions by accounting for the effects of ambient currents on wave propagation and of wave dissipation on alongshore currents and setup.

The video below shows Hurricane Irene's effects in southeast Virginia. Vectors show the wind velocities (m/s), while the contours show the SWAN significant wave heights (m). Irene's track was offshore of Virginia, but large waves propagated toward and into the Chesapeake Bay and threatened portions of Norfolk, Hampton, and neighboring communities.

.. raw:: html

   <iframe width="560" height="315" src="https://www.youtube.com/embed/Xej8eMfEiws?rel=0" frameborder="0" allowfullscreen></iframe>

`SWAN Online Users Manual <https://swanmodel.sourceforge.io/online_doc/swanuse/swanuse.html>`_



