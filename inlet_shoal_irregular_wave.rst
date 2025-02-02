.. _section-inlet-irr:

Irregular wave 
################

.. figure:: images/guide/funwave/eta_inlet_shoal_irr.jpg


Update the wavemaker type and add the appropriate parameters in the wavemaker section of "input.txt" for this example. 

 Set descriptive title for your simulation:

 .. code-block:: rest

        !-----TITLE-----
         TITLE = inlet_irr

 Add an irregular wavemaker with the following conditions:

 .. code-block:: rest

        !-----WAVEMAKER-----
         WAVEMAKER = WK_IRR
         DEP_WK = 10.0
         Xc_WK = 250.0
         Yc_WK = 0.0
         Ywidth_WK = 20000.0
         FreqPeak = 0.0893
         FreqMin = 0.03
         FreqMax = 0.3
         Hmo = 1.00
         GammaTMA = 3.3
         ThetaPeak = 0.0
         Sigma_Theta = 10.0



