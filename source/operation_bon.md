

# Starting the control software in terminal

## Summary

All operations are done with a python script that is lauched using the command:
```
firstpl_controller_start
```
ideally launched in a tmux terminal

## Launching from the tmux

Again, it is highly suggested to start the controller in its own tmux session. This is particularly useful as it allows to control the instrument from an ssh terminal, bypassing the VNC and its sometimes laggy connection.
- `tmux new -s fircam_ctrl` to create the tmux session if it does not exist
- `tmux a -t fircam_ctrl` to connect to the tmux session
- `firstpl_controller_start` to start the controller

## Main classes in software

From there, several objects are defined, which can be used to execute basic commands and pre-defined sequences of commands (aka scripts):
- `cam` contains the methods related to the camera. In particular, most of usual commands from the previous version of the instrument (i.e. `get_tint`, `set_tint`, etc, are available in this object as `cam.get_tint()` etc.).
- `ld` is the lantern driver and contains all the basic commands that can be sent to the electronics.
- `scripts` contains the scripts for the electronics (i.e. sequences of commands for the electronics only).
- `pls` stands for photonic lantern scripts and contains the high-level scripts that interacts with all the elements (i.e. camera, fitslogger, electronics, zabers).
- `pls.focal` is used to control the focal plan camera and the mirror to inject into it
- `zab` is used to control the zabers that move the photonic lantern.

To exit the controller, run:
- `stop()` to stop the different processes and disconnects from the ZMQ ports
- `exit` to leave the ipython terminal

## Initialisation commands

The `pls.bon` object contains two methods to reset the electronics and the fitslogger to a working state. Both startup methods should be called before starting operations. In case any issue occurs during the night, re-running those two methods is also a good first approach to debugging.

To reboot the electronics and reset it to a working state:
```
pls.bon.startup_electronics()
```

To reset the fitslogger:
```
pls.bon.startup_fitslogger()
```

The rolling mode can be activated (see acquiring data):
```
pls.acq.set_mode_rolling(x = 0, y = 0, open_loop = False)
```

To put the system in triggered mode, run:
```
pls.acq.set_mode_triggered()
```

