# Utility tools

## tmux note

If you are processing the data directly on the first machine (kamua), we recommand using tmux:
`tmux new -s first_pipeline`
or if it already exists: 
`tmux a -t first_pipeline`

## $DETDATA alias

You can go to the data directory directly with the command `cd $DETDATA`

## runLP_dfits

It shows the most important parameters of header:
![](FIRST-PL_dfits.png)

Note that it needs dfits to be installed. It can be found there:
https://github.com/granttremblay/eso_fits_tools


## runPL_changeKeyword.py

change the important DPR keywords of files (for exemple, to mark a DARK if not properly taged)
