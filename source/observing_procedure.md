# Observing procedure

Choose the [observing mode](#instrument_principle) corresponding to your science case.

Follow these observing sequences:

## Spectro-astrometry

Record the Photonic Lantern data ***AND*** either the Visible ***or*** IR focal plane.


## On-axis image reconstruction

**In this mode, the calibrator is a different star from the sceince target** <br>

***Choice of calibrator:*** At least as bright as the science target

**Observation procedure:**

1. Point the telescope to the calibrator
2. Acquire data while modulating the tip/tilt mirror
```{image} 1_obs.png
:width: 250 px
```
3. Point the telescope to the science target
4. Acquire data while modulating the tip/tilt mirror
```{image} 2_obs.png
:width: 250 px
```

*Overheads for calibrator :* Setup field + setup AO + acquisition



## Off-axis image reconstruction

**In this mode, the calibrator is the central star, science target is off-axis** <br>

***Choice of calibrator:*** Central star

**Observation procedure:**

1. Point the telescope to the target
2. Acquire data while modulating the tip/tilt mirror on the central star
```{image} 3_obs.png
:width: 250 px
```
3. Acquire data while modulating the tip/tilt with an offset on the science target. *Note:  the offset is done by the tip/ilt mirror*
```{image} 4_obs.png
:width: 250 px
```

*Overheads for calibrator :* enter the command for offset in the instrument control

# Overheads

| Action | Overhead |
| - | - |
| Slewing Telescope | ~<8 minutes> |
| Closing AO3k loop | 60 seconds |
| Closing SCExAO loop | 60 seconds |
| Send light to FIRST-PL | 10 seconds |
| Optimizing injection into FIRST-PL | 2 minutes |
| Start acquisition | < 1 minute |
| Stopping AO3k + SCExAO loops | 10 seconds |