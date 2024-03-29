.. _section-1d-basics:


Basics for model setup
######################

In the directory :code:`/simple_cases/surface_wave_1d/input_files/`, you will find multiple instances of input files that are specific to different variations of this example, e.g., regular ("input\_reg.txt) versus irregular ("input\_irr.txt") wavemaker configuration. It should be noted that the model will accept only the file named "input.txt". Therefore, if you want to switch wavemaker cases, you will need to modify the primary "input.txt" file.

**Computational domain**

.. figure:: images/guide/funwave/layout_1dbeach.jpg
    :width: 600px
    :align: center
    :height: 275px
    :alt: alternate text
    :figclass: align-center




For this example, you will set the following in "input.txt". **Remember that all parameters are case sensitive**.

  If running in parallel, set the number of processors in X and Y:

  .. code-block:: rest

        !-----PARALLEL INFO-----
         PX = 2
         PY = 1 

  Set the bathymetry to match the figure above:

  .. code-block:: rest

        !-----DEPTH-----
         DEPTH_TYPE = SLOPE
         DEPTH_FLAT = 10.0
         SLP = 0.05
         Xslp = 800.0

  
  Send the results to a folder named "output":

  .. code-block:: rest

        !-----PRINT-----
         RESULT_FOLDER = output/
  
  
  Set the dimensions of the domain to 1024 x 3 (need at least 3 points in the y-direction):

  .. code-block:: rest

        !------DIMENSION----
          Mglob = 1024
          Nglob = 3

  Set the total computational time, plot time, and screen intervals to 200.0 s, 10.0 s, and 10.0 s, respectively:

  .. code-block:: rest

        !-----TIME-----
         TOTAL_TIME = 200.0
         PLOT_INTV = 10.0
         SCREEN_INTV = 10.0
          
          
  Set the grid spacing in x and y to 1.0 m:

  .. code-block:: rest

        !------GRID-----
          DX = 1.0
          DY = 1.0
 
  **Wavemaker**

          
  Set the periodic boundary conditions to FALSE:

  .. code-block:: rest

        !-----PERIODIC BOUNDARY CONDITION-----
         PERIODIC = F
  
  Set the sponge layer width to 180.0 m on the left boundary:

  .. code-block:: rest

        !-----SPONGE LAYER-----
         DIFFUSION_SPONGE = F
         FRICTION_SPONGE = T
         DIRECT_SPONGE = T
         Csp = 0.0
         CDsponge = 1.0
         Sponge_west_width = 180.0      ! this line
         Sponge_east_width = 0.0
         Sponge_south_width = 0.0
         Sponge_north_width = 0.0


  **Keep the default values** for the :code:`PHYSICS, NUMERICS, WET-DRY, BREAKING,` and :code:`WAVE AVERAGE` sections. 

  Set the :code:`ETA` and :code:`MASK` output files to TRUE:

  .. code-block:: rest

        !-----OUTPUT-----
         ETA = T
         MASK = T

**Postprocessing**

For postprocessing examples, MATLAB and Python scripts are located in :code:`/simple_cases/surface_wave_1d/postprocessing/`.

