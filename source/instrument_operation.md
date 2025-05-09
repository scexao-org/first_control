# Instrument operation

## 1. Moving photonic lantern

Commands to check and set the Zaber position:
```
first_pl_inj x status
first_pl_inj x goto 98500
first_pl_inj y goto 166500
```

## 2. Stop Piezo Modulation

Commands to stop the piezo modulation and reset its position:
```
ld.switch_modulation_loop(False)
ld.move_piezo(0, 0)
```

Note: There is approximately a factor of 10 between the piezo constraint units and the Zaber units.

## 3. Change camera parameters
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


### 4. Scanning with the Tip-Tilt

Modulation sequences:
- **Number 1**: Fixed position at zero
- **Number 2**: 150 hexagonal (diameter = 16)
- **Number 3**: 595 hexagonal (diameter = 31)
- **Number 4**: 144 rectangular (length = 12)
- **Number 5**: 625 axis (length = 25)

Modulation scale:
- **Lantern modulation**: Scale = 30, sampled at 16 units
- **Piezo modulation**: Scale = 1000, using sequence number 5 (length = 25)

Note: Approximately 1 mas (milliarcsecond) per piezo constraint unit.

`objX` and `objY` represent the position around the tip-tilt zero point. They are not necessarily the center of the photonic lantern.

## 5. Acquiring a Cube

To acquire a cube of images:
```
pls.acq.get_images(nimages, ncubes, mod_sequence, mod_scale, tint, objX, objY)
```
- `nimages`: Number of DITs (must be a factor of the sequence length)
- `ncubes`: Number of cubes to acquire

To display the flux from the most recent FITS file:
```
pls.ins.opti_flux()
```

## 6. Closing down (End of the night)

pls.eon == > to be done...

