# Aligning the lantern

## Moving the photonic lantern with Zabers

The Zaber motors move the lantern physically in the focal plane 

Commands to check and set the Zaber position:
- `zab.get_position()`: retrive the (x, y) position of the zaber 
- `zab.move(x, y)`: move the zaber to x, y absolute position
- `zab.delta_move(dx, dy)`: perform a move of dx and dy relative to current position

Note that all these commands take and return values in units of zaber steps. 

## Acquiring and displaying a scan with the tip/tilt

The tip/tilt mirror can be used to quickly acquire a 2D scan to locate the target and centre the PL. A scan is obtained using:
```
pls.acq.take_acquisition_scan(self, wait_until_done = False, tint = 0.1, mod_scale = 200)
```
The `mod_scale` givens the size of the scan (in mas) and `tint` is the DIT time in seconds.

This function will return immediately. Look at the fitslogger and fitsmerger terminals to know when the scan is done and the fits file is saved.

## Displaying the flux map

To display the flux from the most recent FITS file and retrieve the x, y position of the star:
```
x,y = pls.ins.opti_flux()
```

## Centering the photonic lantern

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
