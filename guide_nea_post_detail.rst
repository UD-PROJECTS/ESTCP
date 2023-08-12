Specification of Output Variables
*************************************

NumberStations: number of station for output. If NumberStations :math:`> 0`, need input i,j in STATION\_FILE
 
DEPTH\_OUT: logical parameter for output depth. T or F. 

U: logical parameter for output :math:`u`. T or F. 

V: logical parameter for output :math:`v`. T or F. 

ETA: logical parameter for output :math:`\eta`. T or F. 

HS: logical parameter for output of significant wave height :math:`H_s`. T or F. 

WFC: logical parameter for output of wave force. T or F. 

WDIR: logical parameter for output of peak wave direction. T or F. 

WBV: logical parameter for output of wave orbital velocity. T or F. 

MASK: logical parameter for output wetting-drying MASK. T or F. 

SourceX: logical parameter for output source terms in :math:`x` direction. T or F. 

SourceY:  logical parameter for output source terms in :math:`y` direction. T or F. 

UV3D: logical parameter for output 3D structure. T or F.

Qstk:   logical parameter for output Stokes mass flux. T or F.

DepDt:  logical parameter for output depth variation rate. T or F.

Qsed:  logical parameter for output sediment transport rate. T or F.



Output
***********************************************
The output files are saved in the result directory defined by RESULT\_FOLDER in INPUT. For outputs in ASCII,  a file name is a combination of variable name and an output series number such eta\_0001, eta\_0002, .... The format  and read/write algorithm are  consistent with a depth file.  Output for stations is a series of numbered files such as sta\_0001, sta\_0002 .... 
