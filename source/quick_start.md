# Quick Start Guide

Use this checklist for a normal observing sequence. For explanations and edge cases, see [Operating FIRST-PL](operation_acquiring.md).

### 1. Start the controller

```text
tmux new -s fircam_ctrl
firstpl_controller_start
```

If the session already exists, attach with:

```text
tmux a -t fircam_ctrl
```

Initialize the instrument:

```python
pls.bon.startup_electronics()
```
and
```python
pls.bon.startup_fitslogger()
```

### 2. Configure the camera

Choose one acquisition mode:

```python
pls.acq.set_mode_rolling(x=0, y=0, open_loop=False)
```

or:

```python
pls.acq.set_mode_triggered()
```

### 3. Check alignment

Run a scan and inspect the flux position (camera should be in triggered mode):

```python
pls.acq.get_acquisition_scan(wait_until_done = True, tint = 0.1, mod_scale = 200)
x_tt, y_tt = pls.ins.opti_flux()
```
and to move the piezo:
```python
x_zab, y_zab = pls.geo.tt_to_zab(x_tt, y_tt)
zab.delta_move(-x_zab, -y_zab)
```

or, the all in one function:

```python
pls.acq.center_PL(tint = 0.1, init_scale = 200, n_iterations = 2)
```

### 4. Acquire data

For rolling mode:

```python
pls.acq.get_images_rolling(nimages=150, ncubes=1, tint=0.05, readout_mode='FAST')
```

For triggered mode:

```python
pls.acq.get_images(ncubes=1, tint=0.05, mod_sequence=2, mod_scale=40, objX=0, objY=0)
```

The `mod_sequence` parameter selects the modulation pattern used during acquisition. In practice, `mod_sequence=2` is the standard choice for the PL camera setup in this guide; other values correspond to different chopping or modulation sequences and are typically used for specific calibration or instrument tests. See the full table of available modulation patterns in [Triggered mode](operation_acquiring.md#triggered-mode). Use the default value unless you have a reason to switch patterns.

### 5. A quick wavelength calibration

It will take 20 minutes of your night time, but it could be interesting to make sure spectral calibration is right.

```python
pls.eon.save_neons(quick=True)
```

### 6. Save and close

Verify that the FITS logger is recording in [Save data](saving_images.md). At the end of the night, take calibrations and follow [Closing down](operation_eon.md).

