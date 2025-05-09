# Instrument operation

# Moving the photonic lantern

The Zaber motors move the lantern physically in the focal plane 

Commands to check and set the Zaber position:
```
first_pl_inj x status
first_pl_inj x goto 98500
first_pl_inj y goto 166500
```

# Changing camera parameters

- Change exposure time : `cam.set_tint()`
- Check exposure time : `cam.get_tint()`
- Change readout mode : `cam.set_readout_mode()` 
    - Options:
        - 'FAST' : < 150 ms
        - 'SLOW' : > 150 ms
- Change crop size : `cam.set_camera_mode`
    - Options:
        - 'FIRSTPL' : For the regular Photonic Lantern mode
        - 'FIRSTPLWFS' : For the Wavefront sensing mode
        - 'FIRSTPLSMF' : For imaging of the SMF
        - 'FULL' : Full frame


# Acquiring a Cube / Scanning with the Tip-Tilt

## Modulation sequences

The modulation can be changed according to 2 pameters. The modulation pattern and the modulation scale

Modulation patterns:
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


## Acquisition command

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

# Stoping Piezo Modulation

Commands to stop the piezo modulation and reset its position:
```
ld.switch_modulation_loop(False)
ld.move_piezo(0, 0)
```

Note: There is approximately a factor of 10 between the piezo constraint units and the Zaber units.

# Closing down (End of the night)

pls.eon == > to be done...

