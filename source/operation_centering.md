# Aligning the lantern on source

## 1. Moving the photonic lantern manually with Zabers

The Zaber motors move the lantern physically in the focal plane 

Commands to check and set the Zaber position:
- `zab.get_position()`: retrive the (x, y) position of the zaber 
- `zab.move(x, y)`: move the zaber to x, y absolute position
- `zab.delta_move(dx, dy)`: perform a move of dx and dy relative to current position

Note that all these commands take and return values in units of zaber steps. 

## 2. Centering with a scan using the tip/tilt

### 2.1 Modulation with the Tip-Tilt

The tip/tilt mirror can be used to quickly acquire a 2D scan to locate the target and centre the PL. A scan is obtained using:
```
pls.acq.get_acquisition_scan(wait_until_done = False, tint = 0.1, mod_scale = 200)
```
The `mod_scale` givens the size of the scan (in mas) and `tint` is the DIT time in seconds.

This function will return immediately. Look at the fitslogger and fitsmerger terminals to know when the scan is done and the fits file is saved.

### 2.2 Displaying the flux map

To display the flux from the most recent FITS file and retrieve the x, y position of the star:
```
x,y = pls.ins.opti_flux()
```

### 2.3 Centering the photonic lantern

To convert the tip/tilt x,y position retrieved from the flux map to zaber coordinates:
```
xzab, yzab = pls.geo.tt_to_zab(x, y)
```

This can be used to recenter the zaber to the correct poisition using a "delta_move":
```
zab.delta_move(-xzab, -yzab)
```

This process can be iterated until proper centering is achieved. There is also a dedicated method to automatically performs these steps:
```
pls.acq.center_PL(tint = 0.1, init_scale = 200, n_iterations = 2)
```
Each iteration will be a zoomed iteration centered on the best position from the previous iteration. The function also takes a verification scan at the end (for a total of n_iterations+1 scans).

## 3. Centering based on the display of the focal plane image

### 3.1 Starting the focal plane camera

Use:
```
pls.focal.start()
```
It will start the focal plane camera and move it into the beam. The command `pls.focal.stop()` is doing the opposite.

## 3.2 Acquire a dataset

Use:
```
pls.focal.get_images_triggered()
```
It will store a bunch of fits files that are then used to find the position of the target.

To display the flux from the most recent FITS file and retrieve the x, y position of the star:
```
x,y = pls.ins.opti_flux_fcam()
```

### 3.3 Centering the photonic lantern

To convert the tip/tilt x,y position retrieved from the flux map to zaber coordinates:
```
xzab, yzab = pls.geo.tt_to_zab(x, y)
```

This can be used to recenter the zaber to the correct poisition using a "delta_move":
```
zab.delta_move(-xzab, -yzab)
```

