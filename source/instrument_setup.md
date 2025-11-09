# Working with tmux shell (for remote control)

`tmux ls`                                 # list tmux sessions <br />
`tmux list-session`                       # list tmux sessions <br />
`tmux a -t {session-name}`                # open a tmux session  <br />
`tmux new -s {session-name}`              # create a new tmux session with a new name  <br />
`tmux rename -t {old-name} {new-name}`    # rename an existing session <br />

Use Ctrl-b and then type d to detach from tmux

# Pre-requisites

########## VERY IMPORTANT : 
AFTER EACH COMPUTER REBOOT, RUN :

`cc-rightafterreboot`

# Starting the real time displays

Launch the displays of the lives (each must be launched in their own terminal) :

## 1. start camera viewer

`firstcam -z 2 &` <br />                         # Start the camera viewer <br />

## 2. Display live flux injection map

Opti flux live, a new image is generated for every new saved cube :

`firstpl_opti_start` : link the shared memory with the saved data<br />
`firstpl_opti_show` : display the content of the shared memory<br />
Reconstructed image live : will reconstruct an image for every frame viewed by the camera using the coupling map located in /mnt/datazpool/PL/calibration_files/<br />

## 3. Display live image reconstruction

`firstpl_rtd_start` : create a reconstruction and saves it inside a shared memory (must be restarted for every new coupling map).<br />
`firstpl_rtd_show` : display the content of the shared memory<br />


A new coupling map can be generated quickly inside /mnt/datazpool/PL/calibration_files/ using : 

`run /home/first/src/firstctrl/first_ctrl/plrtd/quick_cm.py` : (in a python terminal) Arguments to use : 

- --n-latest : int -> the number of cubes to use 
- --modid : int -> the modid used to save the fits
- --modscale : int -> the modscale used to save the fits

The code will look for the n latest files with these parameters and build a coupling map which will be automatically saved in /mnt/datazpool/PL/calibration_files/ for instant use, as well as in /mnt/datazpool/PL/all_coupling_maps/.


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

## 3. Checking the vis block

On scexao@scexso2 computer,  <br />
`vis_block status`

Changing the status, 
`vis_block on`
`vis_block off`

## 4. Flattening the DM (in case of issues)

On the scexao@scexao6 computer:  
`dmflat`

To center the PSF of PALILA, use `Ctrl + Arrow Keys`.

# Running the pipeline

`tmux new -s first_pipeline`
or if it already exists: 
`tmux a -t first_pipeline` <br />
and
`runPL_make_preproc.py --loop=10000` <br />
But you need to have acquired a pixel map before (see pipeline description)