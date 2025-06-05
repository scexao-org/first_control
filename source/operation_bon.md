
# Startup methods

The `pls.bon` object contains two methods to reset the electronics and the fitslogger to a working state. Both startup methods should be called before starting operations. In case any issue occurs during the night, re-running those two methods is also a good first approach to debugging.

To reboot the electronics and reset it to a working state:
```
pls.bon.startup_electronics()
```

To reset the fitslogger:
```
pls.bon.startup_fitslogger()
```