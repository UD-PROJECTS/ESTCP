.. _section-inlet-irr30-brk-ref:

30 deg irregular waves, a submerged breakwater with partial reflection
#######################################################################

.. figure:: images/guide/funwave/eta_inlet_shoal_irr_30deg_brk_abs.jpg



 Set descriptive title for your simulation:

 .. code-block:: rest

        !-----TITLE-----
         TITLE = inlet_irr_30deg_brk_ref


 .. code-block:: rest

        !-----DEPTH-----
         DEPTH_TYPE = DATA
         DEPTH_FILE = DEPTH_FILE = dep_shoal_inlet_brk.txt

         BREAKWATER_FILE = brk_shoal_inlet.txt

 "brk_shoal_inlet.txt" has the same format as the depth file, and the values represent the damping width (like a sponge layer).



  


