.. _section-beach-2d-reg:

Regular wave 30 deg oblique incidence
#####################################

.. figure:: images/guide/funwave/wave_reg_30deg.jpg
    :width: 600px
    :align: center
    :height: 400px
    :alt: alternate text
    :figclass: align-center

The following sections will be modified in "input.txt" for this case. 

  Set a descriptive title for your simulation:

  .. code-block:: rest

        !-----TITLE-----
         TITLE = 2D_beach_30deg
  
  
  .. code-block:: rest
       
       WAVEMAKER = WK_REG
       DEP_WK = 8.0 
       Xc_WK = 150.0 
       Yc_WK = 0.0 
       Tperiod = 8.0 
       AMP_WK = 0.5 
       Theta_WK = 30.0    ! this line
       Delta_WK = 3.0
 
