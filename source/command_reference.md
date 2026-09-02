# Command reference

Use this page as a quick lookup during an observing run. Full procedures are linked from the [Quick start](quick_start.md) and [Operating FIRST-PL](operation_acquiring.md) guides.

## Purpose

Collect the commands most often used to start, configure, acquire, align, track, save, and close FIRST-PL observations.

## Prerequisites

Commands in the Python sections are entered in the `fircam_ctrl` controller terminal. Shell commands are entered in the appropriate SCExAO terminal.

## Controller and hardware

| Task | Command |
| --- | --- |
| Attach to controller | `tmux a -t fircam_ctrl` |
| Start controller | `firstpl_controller_start` |
| Exit IPython | `exit()` |
| Electronics startup | `pls.bon.startup_electronics()` |
| FITS logger startup | `pls.bon.startup_fitslogger()` |
| Read Zaber position | `zab.get_position()` |
| Move Zaber absolutely | `zab.move(x, y)` |
| Move Zaber relatively | `zab.delta_move(dx, dy)` |

## Camera

| Task | Command |
| --- | --- |
| Set integration time | `cam.set_tint()` |
| Read integration time | `cam.get_tint()` |
| Set readout mode | `cam.set_readout_mode()` |
| Set camera crop | `cam.set_camera_mode()` |
| Insert or remove Wollaston | `firstpl_wollaston in/out` |

## Acquisition

| Task | Command |
| --- | --- |
| Rolling mode | `pls.acq.set_mode_rolling(x=0, y=0, open_loop=False)` |
| Triggered mode | `pls.acq.set_mode_triggered()` |
| Rolling acquisition | `pls.acq.get_images_rolling(...)` |
| Triggered acquisition | `pls.acq.get_images(...)` |
| Tip/tilt scan | `pls.acq.get_acquisition_scan(...)` |
| Automatic centering | `pls.acq.center_PL(...)` |

## Alignment and tracking

| Task | Command |
| --- | --- |
| Display flux position | `pls.ins.opti_flux()` |
| Convert tip/tilt to Zaber | `pls.geo.tt_to_zab(x_tt, y_tt)` |
| Synchronize LST | `scripts.set_lstnow(location="subaru")` |
| Update target coordinates | `pls.acq.update_target_coordinates()` |
| Enable tracking offset | `ld.switch_tracking_offset(state=True)` |

## Data and shutdown

| Task | Command |
| --- | --- |
| Save all calibrations | `pls.eon.take_all_calibs()` |
| Save darks | `pls.eon.save_darks()` |
| Save flats | `pls.eon.save_flats()` |
| Save neons | `pls.eon.save_neons()` |

## Verification

After any command that changes instrument state, check the relevant viewer, controller output, or FITS logger before continuing.

## Troubleshooting

See [Troubleshooting](troubleshooting.md) for recovery procedures and [Closing down](operation_eon.md) for end-of-night shutdown.
