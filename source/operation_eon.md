# Closing down (End of the night)

## Don't forget

When done with the PL, don't fogett to issue `superk power off`, `first_pickoff out` and `vis_block in` in the scexao2 terminal. `nps 2 5 off` is to shutdown the flat lamp:
```
ssh scexao2 " superk power off; first_pickoff out; firstpl_pickoff out; vis_block in; nps 2 5 off" 
firstpl_fp out
```

## Darks and flats

Darks and flats can be saved from the fircam_ctrl terminal with :

- `pls.eon.save_darks(block_light_on_the_bench=True)`
- `pls.eon.save_flats()`
These code will look for every parameters of readout mode and exposition time saved during the night and take darks/flats with the corresponding parameters. The exposition time of the flats are set independently.

Alternatively, darks/flats with set parameters can be saved with :

```
pls.eon.save_single_dark(detmod = {str, readout mode value to use}, exptime = {float, exposition time to use}, block_light_on_the_bench=True)
pls.eon.save_single_flat(detmod = {str, readout mode value to use}, exptime = {float, exposition time to use})
Note : block_light_on_the_bench is False by default, setting it to true will block light on other instruments.
```