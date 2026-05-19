# Closing down (End of the night)


## Darks and flats

Darks can be saved from the fircam_ctrl terminal with :
```
pls.eon.save_darks(block_light_on_the_bench=True)
```

Flats can be saved from the fircam_ctrl terminal with :
```
pls.eon.save_flats()
```

To do both, one after the other (before going to bed):
```
pls.eon.take_all_calibs()
```

These codes will look for every parameters of readout mode and exposition time saved during the night and take darks/flats with the corresponding parameters. The exposition time of the flats are set independently.


Alternatively, darks/flats with set parameters can be saved with :
```
pls.eon.save_single_dark(detmod = {str, readout mode value to use}, exptime = {float, exposition time to use}, block_light_on_the_bench=True)
```
```
pls.eon.save_single_flat(detmod = {str, readout mode value to use}, exptime = {float, exposition time to use})
```
Note : block_light_on_the_bench is False by default, setting it to true will block light on other instruments.


## Don't forget

- After logging to sc20 :
    - If you have used the superK: `superk power off`
    - If you have used the FIRST internal calibration unit: `nps 2 5 off` to shutdown the flat lamp, 
    - and `first_pickoff out` to remove the calibration pickup mirror
    - Ending with `firstpl_pickoff out` and `vis_block in` is a good reflex

A quick command to be sure is : 
```
ssh sc20 " superk power off; first_pickoff out; firstpl_pickoff out; vis_block in; nps 2 5 off "
```
