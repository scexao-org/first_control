

# Working with tmux shell (for remote control)

tmux ls                                 # list tmux sessions
tmux list-session                       # list tmux sessions
tmux a -t {session-name}                # open a tmux session 
tmux new -s {session-name}              # create a new tmux session with a new name 
tmux rename -t {old-name} {new-name}    # rename an existing session

# SHM control

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


### Create a new SHM (python code)
map_void          = np.zeros(({width}, {height}), dtype=np.float32)
{shm_var}         = shm('{shm_name}', map_void, location=-1, shared=1)
{shm_var}.set_data({image})

### Display live reconstruction
firstpl_rtd_start			# Start and load the SHM. Use rtd.vmax and rtd.vmin to control color scale on the display.
firstpl_rtd_show			# Display the live


# Pre-requisites (stuff that must be running in the background)

## 0. AFTER EACH COMPUTER REBOOT, RUN :

########## VERY IMPORTANT : 
`cc-rightafterreboot`

## 1. starting the tip-tilt listener

`tmux a -t firstpl_tt_listener`
puis:
`firstpl_tt_listener`

## 2. fusion of fits with modulation pattern:

To merge the FITS files to include the modulation pattern (otherwise, it will be missing):

`tmux firstpl_fitsmerger`
puis
`python fitsmod_merger.py /mnt/datazpool/PL/20250505/firstol/`

# Instrument setup

## Send light to the Photonic Lantern

### 1. Pick off mirror
On scexao2 computer, to know the status of the pick off mirror: <br />
`first_pickoff status`

To put the pick off mirror in: <br />
`first_pickoff in`

To put the pick off mirror out: <br />
`first_pickoff out`
also on the scexao2 computer

### 2. Starting the supercontinuum source (for calibration)

On scexao@scexso2 computer,  <br />
`superk power on`

Changing the intensity, 
`src_flux waymore`
`src_flux wayless`

### 3. flattening the DM (in case of issue)

On scexao@scexao6 computer,  <br />
`dmflat`

To center the PSF of PALILA, use ctrl+arrows.

## Optimization procedure

### 1. Start the process of flux recording
In  /home/first/src/firstctrl/FIRST_photom_control/ run :  <br />
`python first_pl_flux.py`
?????

## Old zaber positionning (moving the photonic lantern itself)

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


## running the software to control the photonic lantern

### 0. Start the script
tmux a -t fircam_ctrl
firstpl_controller_start

cam ==> camera
ld ==> lantern driver (low level)
scripts ==> lantern driver (intermediate level, scripts only for electronics)
pls ==> photonic lantern scripts (high level)


### 1. camera control 


camstart first                          # Starts the FIRST-PL Hamamatsu camera 
firstpl_controller_start		# Replace previous command (camstart first), starts camera, electronics etc
firstcam -z 2 &                         # Start the camera viewer

tmux a -t 

pls.acq.get_images(nimages={nb_pts}, ncubes, mod_sequence={mod_id}, mod_scale={size}, tint) # Take fits following a mod pattern
pls.ins.opti_flux()			# Display flux from most recent fits file saved


### 2. Modulation 

scripts.upload_modulation_sequence(num_id, *pls.mod.{mode}())
xmod, ymod = scripts.retrieve_modulation_sequence(num_id)

### 3. fitsLogger

pls.bon.startup_fitslogger()		# To launch in python, Restart the fits logger



# Additional how-to


############ Start Binning

~/src/firstctrl/FIRST_photom_control/    # Code location
run first_pl_crop.py                     # run the code containing the bin function
pl_b = firstpl_crop()                    # Initialize stuff
pl_b.run_binning(N=12)                   # run binning = 12

first_tcp                                                    # tmux session for the UDP trnasfer 
milk-nettransmit 30201 -T 10.20.30.6 -s firstpl_bin -U       # start the UDP trasnfer