# Tracking an off-axis target

The tip/tilt mirror can be used to observe an off-axis target. In order to do this, the electronics need to track the sky rotation, and thus need to know the local sideral time and the target cooridnates.

## Synchronizing the LST

The LST on the electronics is synchronized to the computer using:
```
scripts.set_lst_now(location = "subaru")
```
This will calculate the LST at the given location using astropy, and send it to the electronics. 

The electronic board does not have a proper LST clock. The LST is therefore emulated using an on-board timer, and drifts slightly. As a consequence, it is a good idea to resynchronize the LST whenever an off-axis target is observed (and not just at the beginning of the night)

## Synchronizing the target coordinates

The target coordinates are synchronized between the telescope and the electronics using:
```
pls.acq.update_target_coordinates()
```
This will read the telescope pointing fro, the redis server and send them to the electronics board (using proper units and formatting).

## Activate the tracking

The tracking is activated using:
```
ld.switch_tracking_offset(state = True)
```

From there on, any offset in the `pls.acq.get_image` command should be given in RA/DEC (in mas) and will be automatically tracked. 
