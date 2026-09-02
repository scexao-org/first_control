# Quick start

Use this checklist for a normal observing sequence. For explanations and edge cases, see [Operating FIRST-PL](operation_acquiring.md).

## Purpose

Start FIRST-PL, prepare the camera and electronics, acquire data, and leave the system ready for the next target.

## Prerequisites

- Confirm that the SCExAO environment and required telemetry services are running.
- Work from a dedicated `tmux` session so the controller remains available over SSH.
- Confirm the target and observing mode before configuring the camera.

## Procedure

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
pls.bon.startup_fitslogger()
```

### 2. Configure the camera

Set the integration time, readout mode, and camera mode as required:

```python
cam.set_tint()
cam.set_readout_mode()
cam.set_camera_mode()
```

Choose one acquisition mode:

```python
pls.acq.set_mode_rolling(x=0, y=0, open_loop=False)
```

or:

```python
pls.acq.set_mode_triggered()
```

### 3. Acquire data

For rolling mode:

```python
pls.acq.get_images_rolling(nimages=150, ncubes=1, tint=0.05, readout_mode='FAST')
```

For triggered mode:

```python
pls.acq.get_images(nimages=271, ncubes=1, tint=0.05, mod_sequence=2, mod_scale=40, objX=0, objY=0)
```

### 4. Check alignment

Run a scan and inspect the flux position:

```python
x_tt, y_tt = pls.ins.opti_flux()
x_zab, y_zab = pls.geo.tt_to_zab(x_tt, y_tt)
zab.delta_move(-x_zab, -y_zab)
```

### 5. Save and close

Verify that the FITS logger is recording in [Save data](saving_images.md). At the end of the night, take calibrations and follow [Closing down](operation_eon.md).

## Verification

Before continuing, confirm that the expected number of images and cubes appears in the FITS logger output and that the target is centered in the flux map.

## Troubleshooting

If the controller, camera, or FITS logger does not respond, use [Troubleshooting](troubleshooting.md) before repeating the acquisition.
