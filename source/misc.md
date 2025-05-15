

# Additional how-to


## SHM Stream control 

milk-streamCTRL                                                     # Shows the various shared memories running (or not :p) 

milk-streamFITSlog -d "/mnt/datazpool/PL/" -z 1000 firstpl pstart   # Start the saving process for the firstpl shm with a default of 1000 im per cube in the specifi
ed directry
FPS_FILTSTRING_NAME="FITS" milk-fpsCTR                              # Open the Fits logger
 - In the fitslogger :
    Shift+r : start the process
    Ctrl+r : stop the process
    Ctrl+e : kill the process 

milk-streamFITSlog -z {nimages} -c {ncubes} {shm_name} on           # Starts saving shm_name for ncubes of nimages  


## Create a new SHM (python code)
map_void          = np.zeros(({width}, {height}), dtype=np.float32) <br />
{shm_var}         = shm('{shm_name}', map_void, location=-1, shared=1) <br />
{shm_var}.set_data({image})  <br />


## Old way to start the camera
camstart first                          # Starts the FIRST-PL Hamamatsu camera  <br />


## Manually changing data type
In a terminal, execute the command line `first_datatype DATA_TYPE`,
with DATA_TYPE being one of the following list:
- "ACQUISITION"
- "BIAS"
- "COMPARISON"
- "DARK"
- "DOMEFLAT"
- "FLAT"
- "FOCUSING"
- "OBJECT"
- "SKYFLAT"
- "STANDARD"
- "TEST"


# Old optimization procedure (using Zabers)

The Zaber motors move the lantern physically in the focal plane 

Commands to check and set the Zaber position:
```
first_pl_inj x status
first_pl_inj x goto 98500
first_pl_inj y goto 166500
```

### 1. Start the process of flux recording
In  /home/first/src/firstctrl/FIRST_photom_control/ run :  <br />
`python first_pl_flux.py`

### 2. Optimization
In  /home/first/src/firstctrl/FIRST_photom_control/ run :<br />
`ipython`  <br />
`run first_pl_optimization_injection_iocam.py`<br />
And then :<br />
`pl_inj.whatyouwant`  <br />

#### 2.1 Take a dark
`pl_inj.acq_dark()`
- Option :
    - `vis_block = True/False` (adding the vis block in/out during dark measurement - check with VAMPIRES instrument when using this block)

#### 2.2 Optimize the injection
`pl_inj.optimization_raster(x0=98997,y0=173268,window_step=1000, channel_opt=0, n_raw=10, npt=19,Target='Your_Target')`

|Injection optimization parameters||
|-|-|
| x0 | x coordinate of the center of the window scanned |
| y0 | y coordinate of the center of the window scanned |
|window_step| size (in step) of the window scanned |
|n_raw| number of frames averaged per position |
|npt| number of samples per window side|
|Target| name of your target|

The coupling maps are saved in /home/first/Documents/FIRST-DATA/FIRST_PL/Optim_maps/
They should look like this : 

| On the bench          |  On-sky |
:-------------------------:|:-------------------------:
| ![](SK_processed.png)  |  ![](HIP84893_processed.png) |

If the optimization is successful, the 2D gaussian fit will appear clearly on the coupling map image. If not, adjust the (x0,y0) corrdinates according to the coupling map shape (carreful, if the dark is bad, this process does not work properly).

