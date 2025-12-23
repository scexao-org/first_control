# Setting up the system

## 1. Overview

To control the FIRST-PL instrument using the tip/tilt miror and its electronics, several processes need to be running. These are:
- the <b>tip/tilt listener</b>, which connects to the USB serial port on which the electronics is connected, and forwards incoming/outgoing data packets to the other processes. 
- the <b>firstpl controller</b>, which opens a terminal, starts the camera, and load all the pre-defined commands and scripts to control the instrument.
- the <b>fitslogger</b>, which saves the incoming frames in fits files needs to be started with a specific name for its FIFO queue, such that other processes can interact with it.
- and finally, the <b>fitsmerger</b>, which monitors the data directory where new fits files are saved, and automatically merges the modulation extension into them.
      
## 2. Starting the tip-tilt listener

Although not absolutely required, it is suggested to start the tip/tilt listener in a dedicated tmux terminal. To to do so, run:
- `tmux new -s firstpl_tt_listener` to create the tmux session if it does not exist
- `tmux a -t firstpl_tt_listener` to connect to the tmux session
- `firstpl_tt_listener` to start the listener

At this point, whenever a command is sent/received from the electronics, a blob of hexdecimal should appear in this terminal. If at any point, the electronics seem to misbehave, check that things are running properly in the listener.

To stop the listener, simply run the following commands in the terminal:
- `listener.stop()` to disconnect from the USB port and ZMQ socket
- `exit` to exit the python terminal


## 3. Starting the fitsmerger

The fitsmerger should be started in its own tmux session:
- `tmux new -s firstpl_fitsmerger` to create the tmux session if it does not exist
- `tmux a -t firstpl_fitsmerger` to connect to the tmux session
- `firstpl_fitsmerger` to start the fitsmerger

The re-synchronize the directory with the fitslogger, you can run `merger.change_target_dir()` from the fitsmerger terminal. 

The fitsmerger will automatically look for new fits files and try to append the modulation table to them. If any error occurs, an error message will be displayed. The fitsmerger also checks that the number of dits in the files that are saved by the logger does match the requested number of frames, and can therefore be used to detect any issue with teh fitslogger.


## 4. Starting the control terminal

Again, it is highly suggested to start the controller in its own tmux session. This is particularly useful as it allows to control the instrument from an ssh terminal, bypassing the VNC and its sometimes laggy connection.
- `tmux new -s fircam_ctrl` to create the tmux session if it does not exist
- `tmux a -t fircam_ctrl` to connect to the tmux session
- `firstpl_controller_start` to start the controller

From there, several objects are defined, which can be used to execute basic commands and pre-defined sequences of commands (aka scripts):
- `cam` contains the methods related to the camera. In particular, most of usual commands from the previous version of the instrument (i.e. `get_tint`, `set_tint`, etc, are available in this object as `cam.get_tint()` etc.).
- `ld` is the lantern driver and contains all the basic commands that can be sent to the electronics.
- `scripts` contains the scripts for the electronics (i.e. sequences of commands for the electronics only).
- `pls` stands for photonic lantern scripts and contains the high-level scripts that interacts with all the elements (i.e. camera, fitslogger, electronics, zabers).
- `zab` is used to control the zabers that move the photonic lantern.

To exit the controller, run:
- `stop()` to stop the different processes and disconnects from the ZMQ ports
- `exit` to leave the ipython terminal


## 5. Preparing the fitslogger 

For use with the tip/tilt module, the fitslogger needs to be started with a specific name for its fifo queue, in order to allow the controller to interact with it. This is done by running the following commands in a terminal: 
- `milk-streamFITSlog -d "/mnt/datazpool/PL/" -z 250 firstpl pstart`

Note that this command is supposedly started by the control software. In order to view the fitslooger, enter:

- `firstpl_fitslogger`

Once in the fitslogger screen, you can use the arrows to move around. Press [F2] to move to the FPS CTRL window, and the [RIGHT] to show the main screen.  

The fitslogger is probably the most important thing to monitor carefully during the night. During the night, in case the fitslogger does not work properly, you should re-run this command. That will reset the fitslogger.



