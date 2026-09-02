# Troubleshooting

Use this page when a normal observing step does not behave as expected. Record the active camera mode, integration time, and last command before restarting a process.

## Purpose

Restore the observing system with the smallest restart that addresses the failure.

## Prerequisites

- Work in the relevant `tmux` session.
- Do not power-cycle hardware while another process is actively writing data.
- Preserve terminal output when reporting a failure.

## Camera viewer is frozen

1. Check stream activity:

   ```text
   milk-streamCTRL
   ```

2. Check the `firstpl_fgrab` `tmux` session.
3. If the camera is not running, restart the control software:

   ```text
   firstpl_controller_start
   ```

4. If the camera still does not run, power-cycle it from an SCExAO2 terminal:

   ```text
   nps 2 7 off
   nps 2 7 on
   ```

5. Wait for the camera to come back, then restart the control software.

## FITS logger is not saving

Check that the saving process was started for the `firstpl` stream:

```text
milk-streamFITSlog -d "/mnt/datazpool/PL/" -z 1000 firstpl pstart
FPS_FILTSTRING_NAME="FITS" milk-fpsCTRL
```

In the FITS logger, use `Shift+r` to start recording and `Ctrl+r` to stop it. Confirm that the output directory contains the new FITS file before proceeding.

## Electronics initialization fails

From the controller terminal, rerun the startup methods:

```python
pls.bon.startup_electronics()
pls.bon.startup_fitslogger()
```

If the problem persists, stop the controller cleanly, restart it in its `tmux` session, and repeat initialization before acquiring data.

## Target is not centered

Run a tip/tilt scan, inspect the most recent flux map, convert the result to Zaber coordinates, and apply a relative correction:

```python
pls.acq.get_acquisition_scan(wait_until_done=False, tint=0.1, mod_scale=200)
x_tt, y_tt = pls.ins.opti_flux()
x_zab, y_zab = pls.geo.tt_to_zab(x_tt, y_tt)
zab.delta_move(-x_zab, -y_zab)
```

For repeated corrections, use:

```python
pls.acq.center_PL(tint=0.1, init_scale=200, n_iterations=2)
```

## Off-axis tracking drifts

Resynchronize the local sidereal time and target coordinates before retrying:

```python
scripts.set_lstnow(location="subaru")
pls.acq.update_target_coordinates()
ld.switch_tracking_offset(state=True)
```

## Verification

After a recovery, confirm that the camera stream is active, the FITS logger is recording, and a short test acquisition produces the expected file.
