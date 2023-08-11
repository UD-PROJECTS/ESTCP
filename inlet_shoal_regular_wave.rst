.. _section-inlet-reg:

Regular wave (baseline case)
############################

.. figure:: images/guide/funwave/eta_inlet_shoal_reg.jpg


 Set a descriptive title for your simulation:

 .. code-block:: rest

        !-----TITLE-----
         TITLE = inlet_reg

 Add a monochromatic wavemaker located 250.0 m from the left boundary that produces a wave of amplitude 1.0 m and period 12.0 s:

 .. code-block:: rest

        !-----WAVEMAKER-----
         WAVEMAKER = WK_REG
         DEP_WK = 10.0 
         Xc_WK = 250.0 
         Yc_WK = 0.0 
         Tperiod = 12.0 
         AMP_WK = 1.0 
         Theta_WK = 0.0 


