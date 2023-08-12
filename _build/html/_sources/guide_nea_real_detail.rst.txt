
Model Bathymetry
****************************
.. figure:: images/guide/nearcom/newriver_xyz.jpg
    :width: 500px
    :align: center
    :height: 400px
    :alt: alternate text
    :figclass: align-center  

Figure: Computational domain for nearcom simulation at New River, SC. Color represents water depth in meters. 

The grid input files are saved in x\_circ.txt, y\_circ.txt, and dep\_circ.txt. 

Model Setup
************************

The model setup follows the simulation in the Beach-Inlet-Shoal case. INPUT.txt includes

 .. code-block:: rest

  $*************************HEADING************************                                           
  $                                                                                                   
  PROJ 'New River Inlet' '01'                                                                        
  $ There are two parts in INPUT                                                                                                  
  $ The first part is SWAN INPUT and the second SHORECIRC INPUT                                                         
  $ SWAN INPUT is based on SWAN 40.51AN.                                                                                      
  $                                                                                             
  $********************SWAN INPUT*************************                                           
  $                                                                                                   
  SET 0 90 0.05 200 3 9.81 1025 1 0.10 CARTESIAN                                                      
  MODE NONSTATIONARY TWODIMENSIONAL                                                                   
  $                                                                                                   
  CGRID CURV 319 339 EXC -99999.0 -99999.0 CIRCLE 36 0.040 0.5 43                                     
  READGRID COOR 1 '../input/xxyy_swan_320x340.txt' 3                                                                          
  $                                                                                                   
  INPGRID BOTTOM CURV 0. 0. 319 339 EXC -99999.0                                                      
  READINP BOTTOM 1 '../input/dep_swan_320x340.txt' 3 0 FREE                                                              
  $INPGRID WIND CURV 0. 0. 199 99 NONSTAT 20081114.160000 60 MI 20091019.080000                       
  $READINP WIND 1 SERIES 'fwind.dat' 3 0 FREE                                                                                                                  
  WIND 0 150                                                                                          
  BOUNDPAR1 SHAPESPEC JONSWP 3.3 PEAK DSPR DEGREES                                                    
  BOUNDPAR2 SEGMENT IJ 0 0 339 0 CONSTANT FILE '../input/wave.txt'           
  $                                                                                                   
  $ physics:                                                                                          
  WCAPping CSM                                                                                        
  BREAKING CONSTANT 1.0 0.73                                                                          
  FRICTION JON                                                                                        
  $TRIAD                                                                                               
  $Gen3                                                                                                
  $ AGROW 0.0015                                                                                      
  $OFF QUAD                                                                                           
  $                                                                                                   
  PROP BSBT                                                                                           
  NUMERIC ACCUR .02 .02 .02 98 STAT 15 0.0                                                            
  $                                                                                                                                                                                                                                                                                                                                                                                     
  TEST 0,0                                                                                            
  COMPUTE NONSTAT 20081114.160000 600 SEC 20081124.160000                                               
  STOP        
  ! ***************WAVE CURRENT INTERATION************************
  ! 1) setup SHORECIRC_RUN or SWAN_RUN
  !    DEFAULT is that both are true
  !    NOTE: you have to provide all input parameters for both SC and SWAN
  !    even if you run only SWAN or SHORECIRC-alone
  ! 2)setup a wave-interaction region bounded by east,west,
  ! south and north edges with a width (number of grid point)
  ! this can avoid nonrealistic forcing output from SWAN
  ! 3)setup wave-current interaction delay time WC_LAG(s)
  SWAN_RUN = T
  SHORECIRC_RUN = T
  WC_BOUND_WEST = 0
  WC_BOUND_EAST = 0
  WC_BOUND_SOUTH = 0
  WC_BOUND_NORTH = 0
  WC_LAG = 0.0
  !*************SHORECIRC_TVD INPUT ******************************
  ! NOTE: all input parameter are capital sensitive
  ! --------------------TITLE-------------------------------------
  ! title only for log file
  TITLE = New River Inlet
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
  DEPTH_TYPE = DATA
  ! Depth file
  ! depth format NOD: depth at node (M1xN1), ELE: depth at ele (MxN) 
  ! where (M1,N1)=(M+1,N+1)  
  DEPTH_FILE = ../input/dep_circ_320x340.txt
  DepthFormat = ELE
  ! if depth is flat and slope, specify flat_depth
  DEPTH_FLAT = 10.0
  if depth is slope, specify slope and starting point
  SLP = 0.07
  Xslp = 10.0

  ! -------------------PRINT---------------------------------
  ! PRINT*,
  ! result folder
  RESULT_FOLDER = output/

  ! ------------------DIMENSION-----------------------------
  ! global grid dimension
  Mglob = 320
  Nglob = 340
  ! ----------------- SC STATIONARY ------------------------
  ! if set SHORECIRC stationary mode, define -DSTATIONARY in Makefile
  ! N_iteration represents number of iteration inside SHORECIRC
  ! WATER_LEVEL_FILE is file contains water levels in every state
  N_ITERATION = 100
  WATER_LEVEL_FILE = ../input/water_level.txt
  ! ----------------- TIME----------------------------------
  ! time: total computational time is decided by SWAN INPUT
  ! here define plot field/station time, and screen output interval 
  ! all in seconds
  PLOT_INTV = 60.0
  PLOT_INTV_STATION = 60.0
  SCREEN_INTV = 6.0
  ! -----------------GRID----------------------------------
  ! if defined curvilinear in Makefile, define x and y files
  ! if defined cori_constant = F, define coriolis file, file size 
  ! should be the same as x and y
  LATITUDE_FILE = ../input/cori_320x340.txt
  ! cartesian grid sizes
   ! curvilinear 
  X_FILE = ../input/x_circ_320x340.txt
  Y_FILE = ../input/y_circ_320x340.txt
  ! --------------- BOUNDARY CONDITIONS
  ETA_CLAMPED = T
  V_CLAMPED = F
  FLUX_CLAMPED = F
  TIDE_FILE = ../input/tide_320x340.txt
  FLUX_FILE = ../input/river.txt
  ! ------------------ WIND ----------------------------
  WindForce = F
  WIND_FILE = ../input/wind.txt
  Cdw = 0.0026
  ! --------------- INITIAL UVZ ---------------------------
  ! INI_UVZ - initial UVZ e.g., initial deformation
  !         must provide three (3) files 
  INI_UVZ = F
  ! if true, input eta u and v file names
  ETA_FILE = z.txt
  U_FILE = u.txt
  V_FILE = v.txt
  ! ----------------WAVEMAKER------------------------------
  !  wave makeer
  ! LEF_SOL- left boundary solitary, need AMP,DEP, LAGTIME
  ! INI_SOL- initial solitary wave, WKN B solution, 
  ! need AMP, DEP, XWAVEMAKER 
  ! INI_REC - rectangular hump, need to specify Xc,Yc and WID
  WAVEMAKER = nothing
  ! solitary wave
  AMP = 1.0
  DEP = 0.78
  LAGTIME = 5.0
  XWAVEMAKER = 400.0
  ! Xc, Yc and WID (degrees) are for rectangular hump with AMP
  Xc = 30.00
  Yc = 30.00
  WID = 5.0
  ! ---------------- PERIODIC BOUNDARY CONDITION ---------
  ! Periodic_x=T:West-East periodic boundary condition
  ! Periodic_y=T:South-North periodic boundary condition  
  ! for SWAN you need number of grid for transition
  !
  PERIODIC_X = F
  PERIODIC_Y = F 
  Num_Transit = 30 
  ! ----------------PHYSICS------------------------------
  ! bottom friction coefficient
  Cd = 0.005
  nu_bkgd = 0.001
  ! ----------------NUMERICS----------------------------
  ! time scheme: runge_kutta for all types of equations
  !              predictor-corrector for NSWE
  ! space scheme: second-order
  !               fourth-order
  ! construction: HLLC
  ! cfl condition: CFL
  ! froude number cap: FroudeCap
  Time_Scheme = Runge_Kutta
  ! spacial differencing
  HIGH_ORDER = FOURTH
  CONSTRUCTION = HLLC
  ! CFL
  CFL = 0.5
  ! Froude Number Cap (to avoid jumping drop in shallow water, set 0.8)
  FroudeCap = 2.0

  ! --------------WET-DRY-------------------------------
  ! MinDepth for wetting-drying
  MinDepth=0.01
  ! -----------------
  ! MinDepthfrc to limit bottom friction
  MinDepthFrc = 0.01
  ! ----------------- MIXING ---------------------------
  ! if use smagorinsky mixing, 
  C_smg = 0.25
  ! --------------- AVERAGE FOR RESIDUAL ---------------
  ! to obtain residual current, use  -DRESIDUAL in makefile
  ! T_INTV_mean (s)
  ! note run time should be longer than T_INTV_mean
  T_INTV_mean = 100.0
  ! ------------------ SEDIMENT ------------------------
  !  have to set -DSEDIMENT in Makefile to include sediment 
  !  module
  !  the following are parameters used for coupling and
  !  sediment formulas
  ! note: give a T_INTV_sed smaller than dt will make coupling
  !       every time step
  T_INTV_sed = 0.0
  Factor_Morpho = 100.0
  D_50 = 0.0002
  D_90 = 0.0002
  por = 0.35
  RHO = 1027.0
  nu_water = 0.00000136
  S_sed = 2.65
  ! Soulsby formula needs
  SOULSBY =  T
  z0 = 0.006
  ! Kobayashi cshore formula needs
  KOBAYASHI = F
  angle_x_beach = 90.0
  eB = 0.002
  ef = 0.01
  a_k = 0.2
  b_k = 0.002
  TanPhi = 0.63
  Gm = 10.0
  frc = 0.015
  Si_c = 0.05
  ! -----------------OUTPUT-----------------------------
  ! stations 
  ! if NumberStations>0, need input i,j in STATION_FILE
  NumberStations = 0
  STATIONS_FILE = ../input/gauges.txt
  ! output variables, T=.TRUE, F = .FALSE.
  DEPTH_OUT = T
  U = T
  V = T
  ETA = T
  HS = T
  PER = T
  WFC = F
  WDIR = T
  Wdis = F
  WBV = F
  Umean = F
  Vmean =F
  ETAmean = F
  MASK = T
  ! end of file   

Outputs
************************
.. figure:: images/guide/nearcom/residual_wave_tide.jpg
    :width: 500px
    :align: center
    :height: 350px
    :alt: alternate text
    :figclass: align-center

Figure: Snapshots of surface elevation, combining wave-induced setup and tidal elevation, and currents. 

