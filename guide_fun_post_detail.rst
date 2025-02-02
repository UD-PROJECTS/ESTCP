
Output files
****************************

The output files are saved in the result directory defined by :code:`RESULT_FOLDER` in "input.txt". Output file names are a combination of variable name and an output series number such as :code:`eta_00001, eta_00002, ....`. The format and read/write algorithm are consistent with a depth file. 

CENTRAL module output
************************

 * :code:`eta_xxxxx`: surface elevation
 * :code:`u_xxxxx`: velocity in the x direction
 * :code:`v_xxxxx`: velocity in the y direction
 * :code:`mask_xxxxx`:  masks for wetting (1) and drying (0)
 * :code:`mask9_xxxxx`:  masks for wetting (1) and drying (0) defined in 9 points
 * :code:`hmax_xxxxx`: maximum surface elevation
 * :code:`hmin_xxxxx`: minimum surface elevation
 * :code:`umax_xxxxx`: maximum velocity
 * :code:`MFmax_xxxxx`: maximum momentum flux
 * :code:`VORmax_xxxxx`: maximum vertical vorticiy
 * :code:`time_xxxxx`: tsunami arrival time
 * :code:`p_xxxxx`: volume flux in the x direction
 * :code:`q_xxxxx`: volume flux in the y direction
 * :code:`nubrk_xxxxx`: breaking induced eddy viscosity (when viscosity breaker is on)
 * :code:`etat_xxxxx`: :math:`\eta_t`
 * :code:`age_xxxxx`: breaking age (in seconds) 
 * :code:`roller_xxxxx`: roller-induced mass flux
 * :code:`U_undertow_xxxxx`: roller-induced extra undertow flux in the x direction
 * :code:`V_undertow_xxxxx`: roller-induced extra undertow flux in the y direction


SEDIMENT output
********************

 * :code:`C_xxxxx`: sediment concentration (g/l)
 * :code:`Pick_xxxxx`: pickup rate
 * :code:`Depo_xxxxx`: deposition rate
 * :code:`DchgS_xxxxx`: depth change in suspended load
 * :code:`DchgB_xxxxx`: depth change in bed load
 * :code:`BedFx_xxxxx`: bedload flux in the x direction
 * :code:`BedFy_xxxxx`: bedload flux in the y direction 
 * :code:`dep_xxxxx`:  depth

Station files
********************

To get time series of surface elevation, u and v at interested locations, you can specify a station file which includes the locations for output. Station input files (e.g., gauges.txt) have size (# of stations) x 2, where column 1 contains Mglob location (unit = grid points) and column 2 contains Nglob location (unit = grid points). In 1D cases, the Nglob location (second column) needs to be set to 1 for all gauges; if set to 0, station files will not be outputted.

  * in "input.txt", add :code:`NumberStations = <integer number of stations>`
  * in "input.txt", :code:`STATIONS_FILE = <file name, e.g., gauges.txt>`
  * format of :code:`STATIONS_FILE`: two integers for grid points in x and y, respectively.


Output for stations is a series of numbered files such as :code:`sta_0001, sta_0002 ....` (NOTE: four digit numbers here vs. five digit numbers for 2D output like :code:`eta_00001`). The output file for stations contains four columns: time, eta, u, and v. 

The version after 3.4 uses a buffer to write out stations to speed up the program on a large scale HPC. The array dimensions for the buffer are (:code:`StationOutputBuffer,NumberStations,4`), where :code:`StationOutputBuffer = 1000` by default. You can also specify StationOutputBuffer in input.txt. An example to set :code:`StationOutputBuffer` is in :code:`/simple_cases/tide_constant/`.

  .. code-block:: rest

     TOTAL_TIME = 200.0 
     PLOT_INTV = 1.0 
     PLOT_INTV_STATION = 0.5 
     SCREEN_INTV = 1.0 
     StationOutputBuffer = 100


ASCII format
****************
   
   The default format is ASCII.  The format and read algorithm are  consistent with a depth file.

   A station file contains four columns, which are values of time (s), eta (m), u (m/s) and v (m/s), respectively. 


BINARY format
****************

   When :code:`FIELD_IO_TYPE = BINARY` is specified in "input.txt", the 2D output files such as :code:`eta_00001, ...` are in the binary format. Here's an example of reading in:
 
   MATLAB:

   .. code-block:: python

          fileID = fopen('eta_00001');

          eta = fread(fileID,[Mglob Nglob],'*double');

          fclose(fileID);

          pcolor(eta),shading flat 

   PYTHON:

   .. code-block:: python

          def readBathyData(outputDir, args):
             """Function that reads in FUNWAVE's bathy data from: dep.out in output directory."""
          
             bathyFileName = os.path.join(outputDir, 'dep.out')

             # read in and convert into 2D numpy array if binary fieldiotype                                                                                                    
             fieldIOType = args.fieldiotype # 'ascii' or 'binary'                                                                                                               

             if fieldIOType == 'ascii':
                bathy = np.loadtxt(bathyFileName)
                (Ny, Nx) = bathy.shape # [Nglob,Mglob]                                                                                                                         

             else:
                Ny = args.nglob    # if not included in args, manually set Nx, Ny
                Nx = args.mglob                                                                                                       
				
                # option 1
                bathy = np.fromfile(bathyFileName).reshape(Ny,Nx)                                                                               
				
                # option 2
                with open(bathyFileName, 'rb') as file: # 'rb' is used to read binary in windows, on linux you can use 'r'
                bathy_file = file.read()
                bathy = np.frombuffer(bathy_file, dtype=np.float32)
                bathy = bathy.reshape(Ny, Nx)                                                                                                    
			    
                # option 3
                fin = open(bathyFileName, mode='rb')
                dataType = np.dtype([('elev', '<f8', Ny*Nx)])
                bathyNotParsed = np.fromfile(fin, dtype=dataType)
                bathy = np.zeros([Ny,Nx])
                for j in range(Nx):
                  for i in range(Ny):
                    bathy[i,j] = bathyNotParsed[0][0][(j*Ny)+i]
                
             return bathy

  **Station files do not have Binary format**. 
