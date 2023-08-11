
Parameters
****************************

.. note:: Examples of model setup are provided in the downloaded package (simpe example folder). If you are a beginner, we strongly suggest trying those examples first and find an example similar to your case. Introduction to those examples can be found at `Simple tests <examples.html>`_.

In this section, we will describe the structure of the "input.txt" file and the input parameters required for each module available in FUNWAVE. Each example in the :code:`/simple_cases/` folder of the downloaded package has a version of the "input.txt" file that is unique to that example. You can refer to the "input.txt" file for each example when reviewing the definitions of parameters described here.

In addition, you will need to create the unique executable needed to run the specific example. This executable is created by the "Makefile". In some cases, multiple examples can be run from the same executable so long as the appropriate flags pertaining to the example are active in the "Makefile". To learn more about makefiles and running FUNWAVE, review the Model Download and Setup page.

********************************

INPUT.txt
******************

An example "input.txt" file is presented below. This file is from the Waves on 1D Slope example, located in :code:`/simple_cases/surface_wave_1d/input_files`. The input file is divided into sections with dashes, where lines with "!" are comments. Brief descriptions of the parameters are listed in each section. Most model parameters are specified in "input.txt", except the parameters specific to a particular module. There parameters will be added to "input.txt". Take a moment to familiarize yourself with the basic structure of the FUNWAVE input file. A comprehensive description of all parameters can be found below.

.. code-block:: rest

        ! INPUT FILE FOR FUNWAVE_TVD 
        ! NOTE: all input parameter are capital sensitive 
        ! --------------------TITLE------------------------------------- 
        ! title only for log file 
         TITLE = regular_1D
        
        ! -------------------PARALLEL INFO----------------------------- 
        !  
        !    PX,PY - processor numbers in X and Y 
        !    NOTE: make sure consistency with mpirun -np n (px*py) 
        !     
         PX = 2
         PY = 1
        
        ! --------------------DEPTH------------------------------------- 
        ! Depth types, DEPTH_TYPE=DATA: from depth file 
        !              DEPTH_TYPE=FLAT: idealized flat, need depth_flat 
        !              DEPTH_TYPE=SLOPE: idealized slope,  
        !                                 need slope,SLP starting point, Xslp 
        !                                 and depth_flat 
         DEPTH_TYPE = SLOPE
        ! if depth is flat and slope, specify flat_depth
         DEPTH_FLAT = 10.0
        ! if depth is slope, specify slope and starting point
         SLP = 0.05
         Xslp = 800.0

        ! -------------------PRINT--------------------------------- 
        ! PRINT*, 
        ! result folder 
         RESULT_FOLDER = output/

        ! ------------------DIMENSION----------------------------- 
        ! global grid dimension 
         Mglob = 1024
         Nglob = 3 

        ! ----------------- TIME---------------------------------- 
        ! time: total computational time/ plot time / screen interval  
        ! all in seconds 
         TOTAL_TIME = 200.0 
         PLOT_INTV = 10.0 
         PLOT_INTV_STATION = 0.5 
         SCREEN_INTV = 10.0 
         PLOT_START_TIME = 100.0

        ! -----------------GRID---------------------------------- 
        ! if use spherical grid, in decimal degrees 
         DX = 1.0 
         DY = 1.0 
        
        ! ----------------WAVEMAKER------------------------------ 
        !  wave maker 
        ! LEF_SOL- left boundary solitary, need AMP,DEP, LAGTIME 
        ! INI_SOL- initial solitary wave, WKN B solution,  
        ! need AMP, DEP, XWAVEMAKER  
        ! INI_REC - rectangular hump, need to specify Xc,Yc and WID 
        ! WK_REG - Wei and Kirby 1999 internal wave maker, Xc_WK,Tperiod 
        !          AMP_WK,DEP_WK,Theta_WK, Time_ramp (factor of period) 
        ! WK_IRR - Wei and Kirby 1999 TMA spectrum wavemaker, Xc_WK, 
        !          DEP_WK,Time_ramp, Delta_WK, FreqPeak, FreqMin,FreqMax, 
        !          Hmo,GammaTMA,ThetaPeak 
        ! WK_TIME_SERIES - fft time series to get each wave component 
        !                 and then use Wei and Kirby 1999  
        !          need input WaveCompFile (including 3 columns: per,amp,pha) 
        !          NumWaveComp,PeakPeriod,DEP_WK,Xc_WK,Ywidth_WK 
         WAVEMAKER = WK_REG 
         DEP_WK = 10.0 
         Xc_WK = 250.0 
         Yc_WK = 0.0 
         Tperiod = 8.0
         AMP_WK = 0.5
         Theta_WK = 0.0
         Delta_WK = 3.0   ! the default is 0.5, set a large number for nonlinear waves
        
        ! ---------------- PERIODIC BOUNDARY CONDITION --------- 
        ! South-North periodic boundary condition 
        ! 
         PERIODIC = F

        ! ---------------- SPONGE LAYER ------------------------ 
        ! need to specify widths of four boundaries and parameters if needed
        ! set width=0.0 if no sponge 
         DIFFUSION_SPONGE = F 
         FRICTION_SPONGE = T 
         DIRECT_SPONGE = T 
         Csp = 0.0 
         CDsponge = 1.0 
         Sponge_west_width =  180.0 
         Sponge_east_width =  0.0 
         Sponge_south_width = 0.0 
         Sponge_north_width = 0.0 

        ! ----------------PHYSICS------------------------------ 
        ! parameters to control type of equations 
        ! dispersion: all dispersive terms 
        ! gamma1=1.0,gamma2=1.0: defalt: Fully nonlinear equations 
        !----------------Friction----------------------------- 
         Cd = 0.0 

        ! ----------------NUMERICS---------------------------- 
        ! time scheme: runge_kutta for all types of equations 
        !              predictor-corrector for NSWE 
        ! space scheme: second-order 
        !               fourth-order 
        ! construction: HLLC 
        ! cfl condition: CFL 
        ! froude number cap: FroudeCap 
         ! HIGH_ORDER = THIRD 
        ! CFL 
         CFL = 0.5 
        ! Froude Number Cap (to avoid jumping drop, set 1.5) 
         FroudeCap = 3.0 

        ! --------------WET-DRY------------------------------- 
        ! MinDepth for wetting-drying 
         MinDepth=0.01 

        ! -------------- BREAKING ----------------------------
         VISCOSITY_BREAKING = T  
         Cbrk1 = 0.65 
         Cbrk2 = 0.35 

        ! ----------------- WAVE AVERAGE ------------------------ 
        ! if use smagorinsky mixing, have to set -DMIXING in Makefile 
        ! and set averaging time interval, T_INTV_mean, default: 20s 
         T_INTV_mean = 240.0 
         STEADY_TIME=480.0 

        ! -----------------OUTPUT----------------------------- 
        ! stations  
        ! if NumberStations>0, need input i,j in STATION_FILE 
         NumberStations = 0
         STATIONS_FILE = gauges.txt 
        ! output variables, T=.TRUE, F = .FALSE. 
         DEPTH_OUT = T 
         ETA = T 
         MASK = T 
         WaveHeight = T 

*************************************

Definitions
*******************

.. note:: All parameter names are case sensitive.

**SPECIFICATION OF TITLE** 

* :code:`TITLE`:    title of your case, only used for log file.

**SPECIFICATION OF PARALLELIZATION** 

*  :code:`PX`:  number of processors to use in X
*  :code:`PY`:  number of processors to use in Y  

 .. note:: :code:`PX` and :code:`PY` must be consistent with the number of processors defined in :code:`mpirun` command, e.g., :code:`mpirun -np n` (where n = px * py). For versions 3.0 or lower, :code:`PX (PY)` should be a common factor of :code:`Mglob(Nglob)`.

**SPECIFICATION OF GRID** 

* :code:`Mglob`: global dimension in x direction.

* :code:`Nglob`: global dimension in y direction.

* :code:`DX`: grid size(m) in x direction.

* :code:`DY`: grid size(n) in y direction.
 
* :code:`DEPTH_TYPE`: depth input type. 

 The program includes several simple bathymetry configurations such as:
 
 * :code:`DEPTH_TYPE = DATA`: from a depth file. 
   
 * :code:`DEPTH_TYPE = FLAT`:  flat bottom, need :code:`DEPTH_FLAT` 
                
 * :code:`DEPTH_TYPE = SLOPE`:  plane beach along x direction. It needs three parameters: slope, :code:`SLP`, slope starting point, :code:`Xslp` and flat part of depth, :code:`DEPTH_FLAT`.

* :code:`DEPTH_FILE`: name of bathymetry file, if :code:`DEPTH_TYPE = DATA`. The file dimensions should be the same as :code:`Mglob x Nglob` with the first point as the south-west corner. The read format in the code is shown below:

  .. code-block:: rest

       DO J=1,Nglob
       
        READ(1,*)(Depth(I,J),I=1,Mglob)
        
       ENDDO
 
* :code:`DEPTH_FLAT`: water depth of flat bottom if :code:`DEPTH_TYPE = FLAT` or :code:`DEPTH_TYPE = SLOPE` (flat part of a plane beach).
 
* :code:`SLP`: slope if :code:`DEPTH_TYPE = SLOPE`.

* :code:`Xslp`: starting x (m) of a slope, if :code:`DEPTH_TYPE = SLOPE`.

* :code:`WaterLevel`: Specify a water level which will be added to the input bathymetry and wavemaker depth such as :code:`DEP_WK` (internal wave generator) and :code:`DepthWaveMaker` (left boundary generator). 

 .. note::   IF you add surge or tide level using :code:`WaterLevel`,  please keep :code:`DEP_WK` the same as the original depth in the depth file because the water level will be automatically added to the model bathymetry. 
 
**SPECIFICATION OF TIME** 

* :code:`TOTAL_TIME`: simulation time in seconds.

* :code:`PLOT_INTV`: output interval in seconds (Note, output time is not exact because adaptive :code:`dt` is used.)

* :code:`SCREEN_INTV`: time interval (s) of screen print. 

* :code:`PLOT_INTV_STATION`: time interval (s) of gauge output.

* :code:`PLOT_START_TIME`: start time for output of field results (s). Default: 0.0

* :code:`DT_fixed`: time step (s) if use fixed DT. But :code:`DT_fixed` will be checked by the CFL condition. IF :code:`DT_fixed` does not satisfy CLF, DT/2, DT/4 ... will be checked until it satisfies CFL. Default is using variable DT based on CFL. 

**SPECIFICATION OF PHYSICS** 

* :code:`DISPERSION`: logical parameter for inclusion of dispersion terms.  T - calculate dispersion, F - no dispersion terms. Default: T.

* :code:`Gamma1`: parameter for linear dispersive terms. 1.0 - inclusion of linear dispersive terms, 0.0 - no linear dispersive terms. Default: 1.0.

* :code:`Gamma2`: parameter for nonlinear dispersive terms. 1.0 - inclusion of nonlinear dispersive terms, 0.0 - no nonlinear dispersive terms. Default: 1.0.

  :code:`Gamma1 = 1.0, Gamma2 = 0.0` for NG's equations.

  :code:`Gamma1 = 1.0, Gamma2 = 1.0` for the fully nonlinear Boussinesq equations.
  
* :code:`Gamma3`: parameter for linear shallow water equations (:code:`Gamma3 = 0.0` for linear SWE). When :code:`Gamma3 = 0.0, Gamma1` and :code:`Gamma2` automatically become zero. Default: 1.0.

* :code:`Beta_ref`:  parameter :math:`\beta` defined for the reference level. :math:`\beta` = -0.531 for NG's and FUNWAVE equations. Default: -0.531.

* :code:`VISCOSITY_BREAKING`: logical parameter for viscous breaking. When this option is selected, :code:`Cbrk1` and :code:`Cbrk2` are needed. Default is shock-capturing type breaking.

 * :code:`Cbrk1`: parameter :math:`C1` in Kennedy et al. (2000). Default: 0.45

 * :code:`Cbrk2`: parameter :math:`C2` in Kennedy et al. (2000). Default: 0.35

 .. note::  :code:`Cbrk1` and :code:`Cbrk2` were re-calibrated in Choi et al. (2018). :math:`C1` is around 0.45 in FUNWAVE-TVD, instead of :math:`C1 = 0.65` used in Kennedy et al. 

* :code:`SWE_ETA_DEP`: ratio of height/depth for switching from Boussinesq to NSWE for shock-capturing breaking. The value is :math:`\sim` 0.80. 

* :code:`ROLLER_EFFECT`: logical parameter for roller effects. If it is set :code:`TRUE`, the roller and undertow effects will be taken into account into the sediment (suspended load) transport processes. 
  
* :code:`FRICTION_MATRIX`: logical parameter for homogeneous and inhomogeneous frction field.  T - inhomogeneous, F - homogeneous. Default: F.

* :code:`FRICTION_FILE`: name of friction file if :code:`FRICTION_MATRIX = T`; the file dimensions should be :code:`Mglob x Nglob` with the first point as the south-west corner. The read format in the code is shown below:

  .. code-block:: rest

       DO J=1,Nglob
       
        READ(1,*)(Cd(I,J),I=1,Mglob)a
        
       ENDDO

* :code:`Cd_fixed`: fixed bottom friction coefficient.

* :code:`SHOW_BREAKING`: logical parameter to calculate breaking index. Note that, if :code:`VISCOSITY_BREAKING` is not selected, breaking is calculated using shock wave capturing scheme. The index calculated here is based on Kennedy et al. (2000). 

* :code:`WAVEMAKER_Cbrk`: breaking parameter inside wavemaker. For some cases, the wave breaks inside the wavemaker. This parameter provides :code:`Cbrk` inside the wavemaker domain. For most of cases, set :code:`WAVEMAKER_Cbrk = Cbrk1` or higher. Default: LARGE.

**SPECIFICATION OF NUMERICS** 

* :code:`Time_Scheme`: stepping option, :code:`Runge_Kutta` or :code:`Predictor_Corrector` (not suggested for this version). Default: :code:`Runge_Kutta`.

* :code:`HIGH_ORDER`: spatial scheme option, :code:`FOURTH` for the fourth-order, :code:`THIRD` for the third-order, and :code:`SECOND` for the second-order (not suggested for Boussinesq modeling).  Default: :code:`FOURTH`. 

* :code:`CONSTRUCTION`: construction method, :code:`HLL` for HLL scheme, otherwise for averaging scheme. Default: HLL.

* :code:`CFL`: CFL number, CFL :math:`\sim` 0.5 (default).

* :code:`FroudeCap`: cap for Froude number in velocity calculation for efficiency. The value could be 1.0 :math:`\sim` 10.0. Default: 3.0

* :code:`MinDepth`: minimum water depth (m) for wetting and drying scheme. Suggestion: :code:`MinDepth = 0.001` for lab scale and 0.01 for field scale. Defaut: 0.01.

* :code:`MinDepthFrc`: merge to :code:`MinDepth` for Version 3.1 or higher. If both :code:`MinDepth` and :code:`MinDepthFrc` are specified, the model takes the Maximum value of :code:`MinDepth` and :code:`MinDepthFrc`. 

**SPECIFICATION OF WAVEMAKER** 

* :code:`WAVEMAKER`: wavemaker type. 

* :code:`WAVEMAKER = INI_REC`: initial rectangular hump 
     need :code:`Xc,Yc` and :code:`WID`

* :code:`WAVEMAKER = LEF_SOL`: left boundary solitary
     need :code:`AMP, DEP`, and :code:`LAGTIME`

* :code:`WAVEMAKER = INI_SOL`: initial solitary wave propagate in +x direction, WKN B solution
     need :code:`AMP, DEP`, and :code:`XWAVEMAKER`

* :code:`WAVEMAKER = INI_OTH`:  other initial distribution specified in the code by users

* :code:`WAVEMAKER = WK_REG`: Wei and Kirby 1999 internal wave maker
      need :code:`Xc_WK, Yc_WK, Ywidth_WK, Tperiod, AMP_WK, DEP_WK, Theta_WK, and Time_ramp` (factor of period)

* :code:`WAVEMAKER = WK_IRR`:  Wei and Kirby 1999 TMA spectrum wavemaker (internal)
      need :code:`Xc_WK, Yc_WK, Ywidth_WK, DEP_WK, Time_ramp, Delta_WK,  FreqPeak, FreqMin,FreqMax, Hmo, GammaTMA` (default: 3.3 ), :code:`ThetaPeak` (default: 0.0), :code:`Nfreq` (default: 45), :code:`Ntheta` (default: 24), :code:`EqualEnergy` (if :code:`TRUE`, this means that the frequency splitting is based on Equal-Energy, otherwise, based on Equal-Frequency space)
           
* :code:`WAVEMAKER = JON_2D`:  JONSWAP spectrum wavemaker (internal)
      need :code:`Xc_WK, Yc_WK, Ywidth_WK,
      DEP_WK, Time_ramp, Delta_WK,  FreqPeak, FreqMin,FreqMax,
      Hmo, GammaTMA` (default: 3.3 ), :code:`ThetaPeak` (default: 0.0), :code:`Nfreq` (default: 45), :code:`Ntheta` (default: 24)
            
* :code:`WAVEMAKER = JON_1D`:  JONSWAP 1D spectrum wavemaker (internal)
      need :code:`Xc_WK, Yc_WK, Ywidth_WK,
      DEP_WK, Time_ramp, Delta_WK,  FreqPeak, FreqMin, FreqMax,
      Hmo, GammaTMA` (default: 3.3 ), :code:`Nfreq` (default: 45)  
            
* :code:`WAVEMAKER = TMA_1D`:  TMA 1D spectrum wavemaker (internal)
      need :code:`Xc_WK, Yc_WK, Ywidth_WK,
      DEP_WK, Time_ramp, Delta_WK,  FreqPeak, FreqMin, FreqMax,
      Hmo, GammaTMA` (Note, still use TMA Gamma, default: 3.3 ), :code:`Nfreq` (default: 45)                                   

* :code:`WAVEMAKER = WK_TIME_SERIES`:
      provide an fft of a time series to get each wave component, and then use Wei and Kirby's (1999) wavemaker. For this internal wavemaker, the wave angle is zero (x direction) for all wave components. Need input :code:`WaveCompFile` (including 3 columns: :code:`per, amp, pha`) and :code:`NumWaveComp, PeakPeriod, DEP_WK, Xc_WK, Ywidth_WK`
 
* :code:`WAVEMAKER = WK_DATA2D`: 2D directional spectrum data specified in :code:`WaveCompFile`. Internal wavemaker. Need :code:`Xc_WK, Yc_WK, DEP_WK, Delta_WK`. 

  Format of :code:`WaveCompFile`:

  .. code-block:: rest

       62  35   - NumFreq NumDir 

       0.0925000011921 - PeakPeriod 

       0.0400 - Freq 

       0.0475 - Freq

       ...

       -0.05  - Dir (degree)

       0.0    - Dir (degree)

       ...

       0.01133044 0.00973217 ... (amplitude,m)

  An example for preparing WaveCompFile can be found in /simple_cases/beach_2d/spectral_data/. It is a case which converts wave data from FRF to the model input. Run the matlab script mk_2d_1d_spec_frf.m to generate a file called wave2d_frf.txt. In input.txt, specify :code:`WaveCompFile = ../spectral_data/MATLAB/wave2d_frf.txt`
  

  The read format in fortran:

  .. code-block:: rest

      OPEN(1,FILE=TRIM(WaveCompFile))

       READ(1,*)NumFreq,NumDir

       ALLOCATE (Amp_Ser(NumFreq,NumDir),  &

          Per_Ser(NumFreq),Theta_Ser(NumDir))

       READ(1,*)PeakPeriod  

       DO J=1,NumFreq

          READ(1,*)Per_Ser(J)  

       ENDDO

       DO I=1,NumDir

          READ(1,*)Theta_Ser(I)

       ENDDO

       DO I=1,NumDir

         READ(1,*)(Amp_Ser(J,I),J=1,NumFreq)

       ENDDO

     ! you dont have to input phase info. The program will skip the phase info
     ! if there is no more data below Amp_Ser
       DO I=1,NumDir

         READ(1,*,END=110)(Phase_2D(J,I),J=1,NumFreq) 

       ENDDO
       
      CLOSE(1)
 
* :code:`WAVEMAKER = LEFT_BC_IRR`: Wavemaker at the left boundary (ghost cells). This type of wavemaker reflects waves at the left boundary. Need :code:`WAVE_DATA_TYPE (DATA, TMA2D, JON2D, JON1D)` and other parameters as the same as in the internal wavemaker. Although it is an irregular wavemaker, it can generate regular waves using :code:`WAVE_DATA_TYPE = DATA` by specifying a single wave component.        
       
* :code:`WAVEMAKER = INI_GAUSSIAN or INI_GAU`: initial Gaussian hump. Need :code:`AMP, Xc, Yc, and WID`.          

  Definitions:

 * :code:`WAVE_DATA_TYPE`: Type of wave data needed for :code:`LEFT_BC_IRR` WaveMaker. It can be DATA or other types used for internal wavemakers

 * :code:`AMP`: amplitude (m) of initial :math:`\eta`, if :code:`WAVEMAKER = INI_REC, WAVEMAKER = INI_SOL, WAVEMAKER = LEF_SOL`.

 * :code:`DEP`: water depth at wavemaker location, if :code:`WAVEMAKER = INI_SOL, WAVEMAKER = LEF_SOL`.

 * :code:`LAGTIME`, time lag (s) for the solitary wave generated on the left boundary, e.g., :code:`WAVEMAKER = LEF_SOL`. 
 
 * :code:`XWAVEMAKER`: x (m) coordinate for :code:`WAVEMAKER = INI\_SOL`.

 * :code:`Xc`: x (m) coordinate of the center of  a rectangular hump if :code:`WAVEMAKER = INI_REC`.

 * :code:`Yc`: y (m) coordinate of the center of  a rectangular hump if :code:`WAVEMAKER = INI_REC`.

 * :code:`WID`: width (m) of  a rectangular hump if :code:`WAVEMAKER = INI\_REC, or INI\_GAU`.

 * :code:`Time_ramp`: time ramp (s) for Wei and Kirby (1999) wavemaker. Default: 0.0.
 
 * :code:`Delta_WK`: width parameter :math:`\delta`  for Wei and Kirby (1999) wavemaker.    Need trial and error, usually, :math:`\delta` =  :math:`1.0 \sim 3.0`.  

 * :code:`DEP_WK`: water depth (m) for Wei and Kirby (1999) wavemaker.

 * :code:`Xc_WK`: x coordinate (m) for Wei and Kirby (1999) wavemaker.

 * :code:`Yc_WK`: y coordinate (m) for Wei and Kirby (1999) wavemaker.

 * :code:`Ywidth_WK`: width (m) in y direction for Wei and Kirby (1999) wavemaker. Default: LARGE (999999.0).

 * :code:`Tperiod`: period (s) of regular wave for Wei and Kirby (1999) wavemaker.

 * :code:`AMP_WK`: amplitude (m) of regular wave for Wei and Kirby (1999) wavemaker.

 * :code:`Theta_WK`: direction (degrees) of regular wave for Wei and Kirby (1999) wavemaker. Note: it may be adjusted if a periodic boundary condition is used. A warning will be given if adjustment is made. 
 
 * :code:`Nfreq`: number of frequency components. Default: 45.

 * :code:`Ntheta`: number of direction components. Default: 24.

 * :code:`FreqPeak`: peak frequency (1/s) for Wei and Kirby (1999) irregular wavemaker.

 * :code:`FreqMin`: low frequency cutoff (1/s) for Wei and Kirby (1999) irregular wavemaker.
 
 * :code:`FreqMax`: high frequency cutoff (1/s) for Wei and Kirby (1999) irregular wavemaker.

 * :code:`Hmo`: Hmo (m) for Wei and Kirby (1999) irregular wavemaker.

 * :code:`GammaTMA`: TMA parameter :math:`\gamma` for Wei and Kirby (1999) irregular wavemaker. :code:`GammaTMA = 3.3` if JONSWAP is used. 

 * :code:`ThetaPeak`: peak direction (degrees) for Wei and Kirby (1999) irregular wavemaker. 

 * :code:`Sigma_Theta`: parameter of directional spectrum for Wei and Kirby (1999) irregular wavemaker.

**SPECIFICATION OF TIDE AND SURGE** 

* :code:`TIDAL_BC_ABS`: logical parameter for tidal absorbing boundary conditions, T - tide and surge at open boundaries, F - no tide or surge.

* :code:`TIDAL_BC_GEN_ABS`: logical parameter for the combined tidal and absorbing-generating boundary conditions, T - ture, F - false.

* :code:`TideBcType`: string parameter for data types. Default: TideBcType = CONSTANT

* :code:`TideWest_ETA`: constant eta value at the WEST boundary.

* :code:`TideWest_U`: constant u value at the WEST boundary, defalut: 0.0. 

* :code:`TideWest_V`: constant v value at the WEST boundary, defalut: 0.0. 

* :code:`TideEast_ETA`: constant eta value at the EAST boundary.

* :code:`TideEast_U`: constant u value at the EAST boundary, defalut: 0.0. 

* :code:`TideEast_V`: constant v value at the EAST boundary, defalut: 0.0.

* :code:`TideSouth_ETA`: constant eta value at the SOUTH boundary.

* :code:`TideSouth_U`: constant u value at the SOUTH boundary, defalut: 0.0. 

* :code:`TideSouth_V`: constant v value at the SOUTH boundary, defalut: 0.0.

* :code:`TideNorth_ETA`: constant eta value at the NORTH boundary.

* :code:`TideNorth_U`: constant u value at the NORTH boundary, defalut: 0.0. 

* :code:`TideNorth_V`: constant v value at the NORTH boundary, defalut: 0.0.

* :code:`TideWestFileName`: file name for WEST boundary data.

* :code:`TideEastFileName`: file name for EAST boundary data.

* :code:`TideSouthFileName`: file name for SOUTH boundary data.

* :code:`TideNorthFileName`: file name for NORTH boundary data. 

**SPECIFICATION OF SPONGE LAYER** 

* :code:`DIRECT_SPONGE`: logical parameter for L-D type sponge, T - sponge layer, F - no sponge layer.

  * :code:`R_sponge`: decay rate in L-D type sponge layer. Its values are between 0.85 :math:`\sim` 0.95. Default: 0.85.

  * :code:`A_sponge`: maximum damping magnitude in L-D type sponge. The value is :math:`\sim` 5.0. Default: 5.0
 
* :code:`FRICTION_SPONGE`: logical parameter for friction type sponge, T - sponge layer, F - no sponge layer.

  * :code:`CDsponge`: The maximum Cd for friction type sponge. Default: 10.0

* :code:`DIFFUSION_SPONGE`: logical parameter for diffusion type sponge, T - sponge layer, F - no sponge layer.
 
  * :code:`Csp`: The maximum diffusion coefficient for diffusion type sponge. Default: 1.0
  
* :code:`Sponge_west_width`: width (m) of sponge layer at west boundary.

* :code:`Sponge_east_width`:   width (m) of sponge layer at east boundary.

* :code:`Sponge_south_width`: width (m) of sponge layer at south boundary.

* :code:`Sponge_north_width`: width (m) of sponge layer at north boundary

* :code:`RESULT_FOLDER`: result folder name, e.g., :code:`RESULT_FOLDER = /Users/tmp/`

**SPECIFICATION OF OUTPUT** 

* :code:`NumberStations`: number of station for output. If NumberStations :math:`> 0`, need input i,j in :code:`STATIONS_FILE`
* :code:`OUTPUT_RES`: integer parameter for output data resolution. e.g. 2 for print every 2 points 
* :code:`DEPTH_OUT`: logical parameter for output depth. T or F. 
* :code:`U`: logical parameter for output u. T or F. 
* :code:`V`: logical parameter for output v. T or F. 
* :code:`ETA`: logical parameter for output :math:`\eta`. T or F. 
* :code:`MASK`: logical parameter for output wetting-drying MASK. T or F. 
* :code:`MASK9`: logical parameter for output MASK9 (switch for Boussinesq/NSWE). T or F. 
* :code:`SourceX`: logical parameter for output source terms in x direction. T or F. 
* :code:`SourceY`:  logical parameter for output source terms in y direction. T or F. 
* :code:`P`:  logical parameter for output of  momentum flux in x direction. T or F. 
* :code:`Q`:  logical parameter for output of  momentum flux in y direction. T or F. 
* :code:`Fx`: logical parameter for output of numerical flux F in x direction. T or F. 
* :code:`Fy`: logical parameter for output of numerical flux F in y direction. T or F. 
* :code:`Gx`: logical parameter for output of numerical flux G in x direction. T or F. 
* :code:`Gy`: logical parameter for output of numerical flux G in y direction. T or F. 
* :code:`AGE`: logical parameter for output of breaking age. T or F. 
* :code:`HMAX`: logical parameter for output of recorded maximum surface elevation . T or F. 
* :code:`HMIN`: logical parameter for output of recorded minimum surface elevation . T or F. 
* :code:`UMAX`: logical parameter for output of recorded maximum velocity . T or F. 
* :code:`VORMAX`: logical parameter for output of recorded maximum vorticity . T or F. 
* :code:`MFMAX`: logical parameter for output of recorded maximum momentum flux . T or F.
* :code:`OUT_Time`: logical parameter for output of recorded tsunami arrival time . T or F. 
* :code:`WaveHeight`: logical parameter for output of wave height, Hsig, Hrms, Havg. T or F.
* :code:`OUT_METEO`: logical parameter for output of pressure field. T or F.
* :code:`ROLLER`: logical parameter for output of roller-induced flux. T or F.
* :code:`UNDERTOW`: logical parameter for output of undertow. T or F.
* :code:`OUT_NU`: logical parameter for output of breaking location. T or F.

Simulation checklist
************************

After the FUNWAVE-TVD model has been downloaded and compiled on either your local machine or remote computing environment (e.g., HPC or AWS Cloud), and you’ve familiarized yourself with the examples in the model repository, you’re ready to begin running numerical wave simulations for your application. If you haven’t yet downloaded and compiled the model, follow the instructions the download page. 

Below is a simple checklist to review before hitting “go” on a simulation. This checklist was designed to reduce the probability of experiencing an error when initiating a new simulation; however, it is not comprehensive of all possible input parameters and input file conditions for all applications.  

#. Input wave conditions are within the valid range of the model.
#. For stability, the ratio of DX and water depth is greater than 1/15. 
#. The number of processors to request on the HPC or local machine matches the product of PX and PY from "input.txt". This number should be a divisor of the number of available processors or cores per compute node (e.g., if 1 node supports 44 processors, -np must be 1, 2, 4, 11, 22, or 44).
#. Cross-reference the name and path of the FUNWAVE executable file to be used for the simulation in the run command or PBS script (if submitting a job on an HPC environment).
#. Cross-reference input file names (input.txt, depth.txt, gauges.txt or stations.txt, friction.txt, etc.) across the working directory, "input.txt", and the PBS script (if submitting a job on an HPC environment).
#. Global input filetypes (e.g., depth.txt and friction.txt) are the same size as the domain (Mglob x Nglob) starting from the south-west corner. If generating files in Python, the bathy array will have Nglob rows and Mglob columns.
#. If using a depth type of FLAT or SLOPE, the DEPTH_FLAT = DEP_WK at the wavemaker. If using a depth type of DATA, ensure that the DEP_WK at XC_WK matches the depth at that location in the "bathy.txt". For stability, it is best to artificially smooth the bathymetry to a constant depth at and around the wavemaker.
#. If transferring files from a Windows to a Linux machine (in an HPC environment, for example), run :code:`dos2unix [filename]` on input files to eliminate any possible Windows return characters (^M) at the end of a line.

If you have experience with running many FUNWAVE-TVD simulations and have recommendations to add to this simple checklist with respect to the Central, Vessel, Sediment, or Meteo modules, please send an email the FUNWAVE user group with the subject “Checklist Recommendations”. We will consider your recommendations and include them on the Wiki as appropriate. We appreciate your support and engagement as the modeling community and the model itself continues to evolve and grow.






