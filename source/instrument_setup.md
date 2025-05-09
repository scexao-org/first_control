# Working with tmux shell (for remote control)

`tmux ls`                                 # list tmux sessions <br />
`tmux list-session`                       # list tmux sessions <br />
`tmux a -t {session-name}`                # open a tmux session  <br />
`tmux new -s {session-name}`              # create a new tmux session with a new name  <br />
`tmux rename -t {old-name} {new-name}`    # rename an existing session <br />

# Pre-requisites

########## VERY IMPORTANT : 
AFTER EACH COMPUTER REBOOT, RUN :

`cc-rightafterreboot`


# Starting scripts (to be started only once)

## 1. starting the tip-tilt listener

`tmux a -t firstpl_tt_listener`
puis:
`firstpl_tt_listener`

## 2. start LP control

`tmux a -t fircam_ctrl` <br />
`firstpl_controller_start` <br />

cam ==> camera <br />
ld ==> lantern driver (low level) <br />
scripts ==> lantern driver (intermediate level, scripts only for electronics) <br />
pls ==> photonic lantern scripts (high level) <br />

# Starting the real time displays

## 1. start camera viewer

`firstcam -z 2 &` <br />                         # Start the camera viewer <br />

### 2. Display live reconstruction

`firstpl_rtd_start`			# Start and load the SHM. Use rtd.vmax and rtd.vmin to control color scale on the display. <br />
`firstpl_rtd_show`			# Display the live <br />

# Starting up (Beginning of the night)

## 1. startup electronics 

`pls.bon.startup_electronics()`		# To launch in python, Restart the electronic <br />

## 1. fitslogger 

Start fits logger communication procedure, in basic terminal: <br />
`FPS_FILTSTRING_NAME="FITS" milk-fpsCTRL -f /milk/shm/milkfifologger`

In case the fitslogger does not work properly, you should run this command. That will reset the fitslogger : <br />
`pls.bon.startup_fitslogger()`		# To launch in python, Restart the fits logger <br />


## 2. fusion of fits with modulation pattern:

To merge the FITS files to include the modulation pattern (otherwise, it will be missing):

`tmux a -t firstpl_fitsmerger` <br />
and
`firstpl_fitsmerger` <br />

It must be run AFTER the `startup_fitslogger` command, since it requires parameters (including the name of the directory where the files are stored) <br />


# Send light to the Photonic Lantern

## 1. Pick off mirror
On scexao2 computer, to know the status of the pick off mirror: <br />
`first_pickoff status`

To put the pick off mirror in: <br />
`first_pickoff in`

To put the pick off mirror out: <br />
`first_pickoff out`
also on the scexao2 computer

## 2. Starting the supercontinuum source (for calibration)

On scexao@scexso2 computer,  <br />
`superk power on`

Changing the intensity, 
`src_flux waymore`
`src_flux wayless`

## 3. Flattening the DM (in case of issues)

On the scexao@scexao6 computer:  
`dmflat`

To center the PSF of PALILA, use `Ctrl + Arrow Keys`.


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
