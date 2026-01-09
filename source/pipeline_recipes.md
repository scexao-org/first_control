# Recipes & Usage

## runPL_dfits
Shell script to quickly inspect the key parameters of a FIRST FITS file.  
**Requirements**: `dfits` from [ESO FITS Tools](https://github.com/granttremblay/eso_fits_tools)

### Usage
```bash
./runPL_dfits <path_to_fits_file>
```

---

## runPL_changeKeyword.py
Python script to modify FITS header keywords for FIRST Pipeline classification and processing control.  
Essential tool for classifying files and tracking their processing stages throughout the sequential pipeline workflow. Used for temporary keyword changes during data organization and to ensure proper file selection by downstream scripts.

### Usage
```bash
python runPL_changeKeyword.py [options] [files...]

# Examples:
python runPL_changeKeyword.py --DATA-TYP=FLAT --X_FIRTYP=RAW *.fits
python runPL_changeKeyword.py --OBJECT="HD 164461" --X_FIRTYP=PREPROC target_data/*.fits
python runPL_changeKeyword.py --DATA-TYP=COMPARAISON --X_FIRTYP=RAW neon_calib.fits
python runPL_changeKeyword.py --DATE=DEFAULT --X_FIRTYP=RAW recent_observations/*.fits
```

### Key Options
- `--DATA-TYP`: Data classification (FLAT=SuperK data, DARK=background, OBJECT=science targets, ACQUISITION=target acquisition, COMPARAISON=Neon calibration, TEST=validation)
- `--X_FIRTYP`: Processing stage (RAW=unprocessed, PREPROC=preprocessed, PIXELMAP/WAVEMAP/COULPLINGMAP=calibration products)
- `--OBJECT`: Target name for science observations (e.g., "HD 164461", "Beta Pic")
- `--X_FIRMID`: Modulation ID identifying specific modulation pattern
- `--X_FIRTRG`: Camera trigger mode (INT=internal, EXT=external synchronization)
- `--X_FIRWOL`: Wollaston prism status (IN=polarimetry mode, OUT=photometry mode)
- `--GAIN`: Camera gain setting value
- `--DATE`: Observation date (use DEFAULT to extract from filename)


---

## runPL_create_pixelMap.py
Python script to generate pixel maps essential for FIRST Pipeline spectral trace alignment and calibration.  
Pixel maps detect and calibrate the positions of spectral traces across all fiber channels, enabling proper spectral extraction in downstream processing.

### Usage
```bash
python runPL_create_pixelMap.py [options] [file_patterns...]

# Examples:
python runPL_create_pixelMap.py --pixel_min=100 --pixel_max=1600 --pixel_wide=2 --filter_files *.fits
python runPL_create_pixelMap.py --pixel_min=50 --pixel_max=1500 data/*.fits
python runPL_create_pixelMap.py --filter_files /data/raw/*.fits
```

### Key Options
- `--pixel_min`: Minimum pixel value along wavelength axis (default: 100)
- `--pixel_max`: Maximum pixel value along wavelength axis (default: 2100)  
- `--pixel_wide`: Window half width for peak detection (default: 2)
- `--filter_files`: Quality control to exclude low-flux files for reliable detection

### Pipeline Integration
- Processes RAW files to create pixel alignment maps
- Essential first step before any spectral analysis
- Output maps used by runPL_make_preproc.py for spectral extraction
- Automatically handles different Wollaston configurations (38 vs 19 channels)

### Input
Files with `X_FIRTYP=RAW`

### Output
Pixel map FITS file and PNG visualization in `pixelmaps/` directory

---

## runPL_make_preproc.py
Python script to preprocess raw FIRST Photonic Lantern data using pixel maps for spectral extraction and calibration.  
Transforms raw detector images into calibrated spectral data with quality assessment and diagnostic analysis.

### Usage
```bash
python runPL_make_preproc.py [options] [files...]

# Examples:
python runPL_make_preproc.py --pixel_map=/path/to/pixel_map.fits /path/to/directory
python runPL_make_preproc.py --object="HD 164461" /path/to/files*.fits
python runPL_make_preproc.py --loop 30 /path/to/directory  # Monitor mode
```

### Key Options
- `--pixel_map`: Specify pixel map file (auto-detected if not provided)
- `--loop`: Monitor directory and process new files every X seconds (real-time mode)
- `--object`: Process only files with specified OBJECT name

### Pipeline Integration
- Requires raw files (X_FIRTYP=RAW) and pixel maps (X_FIRTYP=PIXELMAP)
- Essential step before flat field, wavelength, and coupling map generation
- Quality metrics guide downstream data acceptance decisions

### Input
Raw files with `X_FIRTYP=RAW` and pixel maps with `X_FIRTYP=PIXELMAP`

### Output
Preprocessed files with `X_FIRTYP=PREPROC` in `preproc/` directory plus diagnostic figures

---

## runPL_create_flatMap.py
Python script to generate flat field calibration maps from SuperK data for FIRST Pipeline photometric correction.  
Creates gain coefficients and quality metrics for pixel-to-pixel sensitivity correction using linear regression analysis.

### Usage
```bash
python runPL_create_flatMap.py [options] [files...]

# Examples:
python runPL_create_flatMap.py --wollaston IN --dark_files=dark*.fits flat_data/*.fits
python runPL_create_flatMap.py --dark_files=/path/to/darks/*.fits *.fits
```

### Key Options
- `--wollaston`: Wollaston status (IN for polarimetry, OUT for photometry)
- `--dark_files`: Select specific dark file(s) for background subtraction

### Pipeline Integration
- Essential calibration step before coupling map generation
- Uses preprocessed flat field and dark files
- Output maps enable photometric correction in downstream analysis

### Input
Preprocessed flat field files with `X_FIRTYP=PREPROC` and `DATA-TYP=FLAT`

### Output
Flat field maps with gain coefficients and quality metrics in `flatmaps/` directory

---

## runPL_create_waveMap.py
Python script to generate wavelength calibration maps from Neon emission line spectra for FIRST Pipeline spectral calibration.  
Detects emission lines, fits polynomial wavelength solutions, and generates 2D wavelength mapping with aberration correction.

### Usage
```bash
python runPL_create_waveMap.py [options] [files...]

# Examples:
python runPL_create_waveMap.py --wollaston IN --flatMap=/path/to/flat.fits *.fits
python runPL_create_waveMap.py --Nexclude 3 --dark_files=dark*.fits neon_data/*.fits
```

### Key Options
- `--wollaston`: Wollaston status (IN for polarimetry, OUT for photometry)
- `--flatMap`: Select specific flat map file for enhanced calibration
- `--dark_files`: Select specific dark file(s) for background subtraction
- `--Nexclude`: Number of wavelength peaks to exclude from fit for outlier rejection (default: 4)

### Pipeline Integration
- Uses Neon calibration files and flat field maps for accurate calibration
- Essential for spectral analysis in downstream scripts
- Output maps enable precise wavelength calibration of science observations

### Input
Neon calibration files with `X_FIRTYP=PREPROC` and `DATA-TYP=COMPARISON`

### Output
Wavelength map with polynomial coefficients and aberration correction in `output/wave/` directory

---

## runPL_create_couplingMap.py
Python script to generate coupling efficiency maps from preprocessed FIRST Photonic Lantern data using SVD analysis.  
Analyzes coupling efficiency between telescope focal plane and photonic lantern channels, essential for image reconstruction.

### Usage
```bash
python runPL_create_couplingMap.py [options] [files...]

# Examples:
python runPL_create_couplingMap.py --object_name="HD 164461" --wavelength_smooth=7 *.fits
python runPL_create_couplingMap.py --modID=1 --modScale=2 --wollaston=IN data/*.fits
python runPL_create_couplingMap.py --flatMap=/path/to/flat.fits --waveMap=/path/to/wave.fits *.fits
```

### Key Options
- `--object_name`: Select specific science target for processing
- `--wavelength_smooth`: Smoothing factor for wavelength processing (default: 7)
- `--wavelength_bin`: Binning factor for wavelength (default: 20)
- `--Nsingular`: Number of SVD singular values to retain (default: 19*6)
- `--modID/modScale`: Choose specific modulation patterns
- `--wollaston`: Wollaston status (IN for polarimetry, OUT for photometry)

### Pipeline Integration
- Requires preprocessed data, flat field maps, and wavelength calibration
- Critical step for converting fiber measurements to sky coordinates
- Output enables image reconstruction and astrometric analysis

### Input
Preprocessed files with `X_FIRTYP=PREPROC`

### Output
Coupling maps with `X_FIRTYP=COUPLINGMAP` in `../couplingmaps/` directory plus PDF diagnostics

---

## runPL_make_image.py
Python script to reconstruct astronomical images from FIRST Photonic Lantern fiber measurements using coupling map inversion.  
Transforms fiber-based measurements into traditional astronomical images for conventional analysis techniques.

### Usage
```bash
python runPL_make_image.py [options] [files...]

# Examples:
python runPL_make_image.py --object_name="HD 164461" --wavelength_smooth=7 *.fits
python runPL_make_image.py --coupling_map=/path/to/map.fits --modID=1 data/*.fits
python runPL_make_image.py --save_individual_frames --save_individual_wavelength *.fits
```

### Key Options
- `--object_name`: Select specific target for reconstruction
- `--coupling_map`: Force selection of specific coupling map file
- `--wavelength_smooth`: Control spectral smoothing for noise reduction (default: 7)
- `--modID/modScale`: Choose optimal modulation patterns
- `--save_individual_frames`: Generate time-resolved image sequences (default: True)
- `--save_individual_wavelength`: Create spectral image cubes (default: False)
- `--wollaston`: Wollaston status (IN for polarimetry, OUT for photometry)

### Pipeline Integration
- Final step: converts fiber measurements to spatial images
- Enables conventional image analysis of photonic lantern data
- Results comparable with traditional imaging instruments

### Input
Coupling maps and preprocessed data cubes

### Output
Reconstructed images, residuals, and diagnostic plots with optional frame sequences

---

## runPL_make_astrometry.py
Python script to perform high-precision astrometric measurements from FIRST Photonic Lantern data using coupling map analysis.  
Enables sub-milliarcsecond position measurements for binary stars, exoplanet detection, and precision astrometry applications.

### Usage
```bash
python runPL_make_astrometry.py [options] [files...]

# Examples:
python runPL_make_astrometry.py --wollaston IN --wavelength_smooth=2 *.fits
python runPL_make_astrometry.py --coupling_map=/path/to/map.fits --pyramids target_data/*.fits
python runPL_make_astrometry.py --save_individual_frames --save_individual_wavelength *.fits
```

### Key Options
- `--wollaston`: Wollaston status (IN for polarimetry, OUT for photometry)
- `--coupling_map`: Force selection of specific coupling map file
- `--dark_files`: Select specific dark file(s) for background subtraction
- `--wavelength_smooth`: Smoothing factor for position determination (default: 1)
- `--pyramids`: Enable pyramidal fitting for enhanced spatial resolution (default: False)
- `--save_individual_frames`: Generate time-resolved astrometric sequences (default: True)
- `--save_individual_wavelength`: Analyze chromatic astrometric effects (default: False)

### Pipeline Integration
- Final analysis step for precision position measurements
- Leverages photonic lantern spatial resolution enhancement
- Enables astrometry beyond conventional imaging limits

### Input
Preprocessed FITS files with coupling maps

### Output
Astrometric measurements with sub-milliarcsecond precision, quality metrics, and uncertainty estimates
