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

## 1. start camera viewer

`firstcam -z 2 &` <br />                         # Start the camera viewer <br />

## 2. Display live reconstruction

`firstpl_rtd_start`			# Start and load the SHM. Use rtd.vmax and rtd.vmin to control color scale on the display. <br />
`firstpl_rtd_show`			# Display the live <br />

## 2. Other live Displays

...

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

# Running the pipeline

`tmux a -t firstpl_pipeline` <br />
and
`runPL_preprocess.py --loop=10000` <br />
But you need to have acquired a pixel map before (see pipeline description)