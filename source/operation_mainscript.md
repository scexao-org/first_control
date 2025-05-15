# fircam_ctrl

`tmux a -t fircam_ctrl` <br />
`firstpl_controller_start` <br />

cam ==> camera <br />
ld ==> lantern driver (low level) <br />
scripts ==> lantern driver (intermediate level, scripts only for electronics) <br />
pls ==> photonic lantern scripts (high level) <br />
zab ==> control the zabers that moves the photonic lantern  <br />

## Moving the photonic lantern with Zabers

The Zaber motors move the lantern physically in the focal plane 

Commands to check and set the Zaber position:
```
zab.get_positions()
zab.set_positions()
zab.delta_move(x,y)
```

## Changing camera parameters

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

## Swithcing mode between triggered or rolling

To set mode trigger:
`pls.acq.set_mode_triggered()`
To set mode rolling:
`pls.acq.set_mode_rolling(x,y)`
where x and y are the position of the piezo.


## get_image command

### Parameters

To acquire a cube of images:
```
pls.acq.get_images(nimages, ncubes, tint, mod_sequence, mod_scale, objX, objY)
```
- `nimages`: Number of DITs (must be a factor of the sequence length)
- `ncubes`: Number of cubes to acquire
- `tint`: Integration time of the camera
- `mod_sequence`: See numbers above (must be between 1 and 5)
- `mod_scale`: See numbers above

### Choosing a modulation sequence

The modulation can be changed according to 2 pameters. The modulation pattern and the modulation scale

Modulation patterns:
- **Number 1**: Fixed position at zero
- **Number 2**: 150 hexagonal (diameter = 16)
- **Number 3**: 595 hexagonal (diameter = 31)
- **Number 4**: 144 rectangular (length = 12)
- **Number 5**: 625 rectangular (length = 25)

Modulation scale:
- **Lantern modulation**: Scale = 30, sampled at 16 units
- **Piezo modulation**: Scale = 1000, using sequence number 5 (length = 25)

Note: Approximately 1 mas (milliarcsecond) per piezo constraint unit.

## Displaying the flux map

To display the flux from the most recent FITS file (folliwing a get_images):
```
x,y = pls.ins.opti_flux()
```

## Centering the photonic lantern

To convert the x,y position for the flux map :
```
xzab, yzab = pls.geo.tt_to_zab(x, y)
```
To send the zaber to correct poisition using a "delta_move" from actual position:
```
zab.delta_move(-xzab, -yzab)
```

## Stoping Piezo Modulation

Commands to stop the piezo modulation and reset its position:
```
ld.switch_modulation_loop(False)
ld.move_piezo(0, 0)
```

Note: There is approximately a factor of 10 between the piezo constraint units and the Zaber units.

