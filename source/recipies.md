# Pipeline overview

![](overview.png) 

# Recipes

## runPL_createPixelMap.py

**Usage:**
```
runPL_createPixelMap.py [options] [file_patterns]
```
**Goal:** Create the pixel map needed to preprocess the data.

**Arguments:**
- `file_patterns`: One or more glob patterns for FITS files (default: *.fits)

**Input:**
- FITS files with `X_FIRTYP=RAW` in the directory.

**Output:**
- A FITS file with the pixel map.
- A PNG file with the pixel map visualization.

**Options:**
- `--pixel_min`         Minimum pixel value along wavelength axis (default: 20)
- `--pixel_max`         Maximum pixel value along wavelength axis (default: 1600)
- `--pixel_wide`        Window half width (default: 2; full width = 2*pixel_wide+1)
- `--output_channels`   Number of output channels (default: 38)

**Examples:**
```
runPL_createPixelMap.py --pixel_min=20 --pixel_max=1600 --pixel_wide=2 --output_channels=38 *.fits
runPL_createPixelMap.py --pixel_min=50 --pixel_max=1500 --output_channels=32 data/*.fits
```

## runPL_preprocess.py

**Usage:**
```
runPL_preprocess.py [options] [directory | files.fits]
```
**Goal:** Preprocess the data using the pixel map.

**Input:**
- FITS files with `X_FIRTYP=RAW` in the directory.
- FITS files with `X_FIRTYP=PIXELMAP` in the directory.
- The pixel map file is used to extract the data from the raw files and create new FITS files.

**Output:**
- FITS files with `X_FIRTYP=PREPROC` in the preproc directory.
- Diagnostic figures saved in the preproc directory:
    * Pixel map overlay on raw images.
    * Centroid shift of the data in the pixel map as a function of time.
- Pixel shift information is also stored in the FITS header (`QC_SHIFT`).

**Options:**
- `--pixel_map=FILE`   Specify which pixel map FITS file to use (default: auto-detect in directory)
- `--loop=SECONDS`     Loop and check for new files every X seconds (default: 0, i.e., run once)

**Examples:**
```
runPL_preprocess.py --pixel_map=/path/to/pixel_map.fits /path/to/directory
runPL_preprocess.py /path/to/files*.fits
```

**Notes:**
- The centroid shift figure is useful to check if the position of the pixels changed over time.

## runPL_createWavelengthMap.py

**Usage:**
```
runPL_createWavelengthMap.py [options]
```
**Goal:** Create a wavelength map from the provided FITS files.

**Summary:**
- Searches for FITS files with `X_FIRTYP=PREPROC` and `DATA-TYP=WAVE` keywords.
- Finds corresponding dark files (`X_FIRTYP=PREPROC`, `DATA-TYP=DARK`).
- Reads wave files, subtracts the median of the dark files.
- Detects emission peaks and fits a polynomial to create a wavelength map.
- The number of peaks (N) is determined by the number of wavelengths in `--wave_list`.
- Saves the wavelength map as a FITS file in the output directory.
- Generates and saves figures for visualization.
- Output files are stored in an `output/wave` directory.

**Options:**
- `--wave_list`   Comma-separated list of emission lines (default: [748.9, 743.9, 724.5, ...])
- `--filelist`    Folder containing the preprocessed FITS files (default: .)

**Example:**
```
runPL_createWavelengthMap.py --wave_list="[748.9, 743.9, 724.5, ...]"
```

## runPL_createCouplingMap.py

**Usage:**
```
runPL_createCouplingMap.py [options] files.fits
```
**Goal:** Create coupling maps from preprocessed photonic lantern data.

**Summary:**
- Takes as input a list of files with `DPR_CATG=CMAP` and `DPR_TYPE=PREPROC` keywords.
- Selects files based on modulation pattern, modulation scale, and object name if specified.
- Computes SVD-based coupling maps, saves results to FITS, and generates diagnostic plots.

**Input:**
- FITS files with `X_FIRTYP=PREPROC` in the directory or matching the argument pattern.

**Output:**
- FITS files with `X_FIRTYP=COUPLINGMAP` in the `../couplingmaps` directory.
- A PDF report with plots of the coupling maps and the SVD analysis.

**Options:**
- `--wavelength_smooth`  Smoothing factor for wavelength (default: 20)
- `--wavelength_bin`     Binning factor for wavelength (default: 15)
- `--object_name`        Select data by object name (default: NONE)
- `--modID`              Select modulation pattern by user [0 = first in list] (default: 0)
- `--modScale`           Select modulation scale by user [0 = first in list] (default: 0)
- `--Nsingular`          Number of singular values to use (default: 57)

**Example:**
```
runPL_createCouplingMap.py *.fits
```

## runPL_imageReconstruction.py

**Goal:**
Reconstruct images from FIRST Photonic Lantern data using specified coupling maps and options.

**Summary:**
- Processes preprocessed FITS files, applies coupling maps, reconstructs images, and saves the results.
- Supports selection by object name, modulation pattern, and smoothing options.
- Can save individual frames, wavelength slices, and residuals, and allows explicit selection of coupling map files.

**Input:**
- Preprocessed data FITS files (e.g., with `DPR_CATG=OBJECT` and `DPR_TYPE=PREPROC`)
- Coupling map FITS files (e.g., with `X_FIRTYP=COUPLINGMAP`)

**Output:**
- Reconstructed image FITS files, including summed images, residuals, and optionally individual frames and wavelength slices.

**Options:**
- `--object_name <str>`           Select data by object name (default: NONE)
- `--modID <int>`                 Select modulation pattern by user [0 = first in list] (default: 0)
- `--modScale <int>`              Select modulation scale by user [0 = first in list] (default: 0)
- `--coupling_map <str>`          Select which coupling map file to use (default: the one in the directory)
- `--wavelength_smooth <int>`     Smoothing factor for wavelength (default: 1)
- `--save_individual_frames`      Save individual frames (default: True)
- `--save_individual_wavelength`  Save individual wavelength slices (default: False)

**Example:**
```
python runPL_imageReconstruction.py --object_name=HIP81126 --modID=1 --coupling_map=path/to/couplingmap.fits *.fits
```
