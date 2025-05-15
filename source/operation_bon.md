
# Starting up (Beginning of the night)

## 1. starting the tip-tilt listener

`tmux a -t firstpl_tt_listener`
puis:
`firstpl_tt_listener`

## 3. start LP control

`tmux a -t fircam_ctrl` <br />
`firstpl_controller_start` <br />

cam ==> camera <br />
ld ==> lantern driver (low level) <br />
scripts ==> lantern driver (intermediate level, scripts only for electronics) <br />
pls ==> photonic lantern scripts (high level) <br />
zeb ==> control the zabers that moves the photonic lantern  <br />

## 4. startup electronics 

`pls.bon.startup_electronics()`		# To launch in python, Restart the electronic <br />

## 5. fitslogger 

Start fits logger communication procedure, in basic terminal: <br />
`FPS_FILTSTRING_NAME="FITS" milk-fpsCTRL -f /milk/shm/milkfifologger`

And then run the command: <br />
`pls.bon.startup_fitslogger()`		# To launch in python, Restart the fits logger <br />

During the night, in case the fitslogger does not work properly, you should re-run this command. That will reset the fitslogger

## 6. fusion of fits with modulation pattern:

To merge the FITS files to include the modulation pattern (otherwise, it will be missing):

`tmux a -t firstpl_fitsmerger` <br />
and
`firstpl_fitsmerger` <br />

It must be run AFTER the `startup_fitslogger` command, since it requires parameters (including the name of the directory where the files are stored) <br />
