# Camera control

## Change camera parameters
Enter in the tmux session `fircam_ctrl`
- Change exposure time : `set_tint()`
- Check exposure time : `get_tint()`
- Change readout mode : `set_readout_mode()` 
    - Options:
        - 'FAST' : < 100 ms
        - 'SLOW' : > 100 ms
- Change crop size : `set_camera_mode`
    - Options:
        - 'FIRSTPL' : For the regular Photonic Lantern mode
        - 'FIRSTPLWFS' : For the Wavefront sensing mode
        - 'FIRSTPLSMF' : For imaging of the SMF
        - 'FULL' : Full frame

## Change data type
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
