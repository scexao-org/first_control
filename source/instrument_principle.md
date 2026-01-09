# Instrument presentation

The Fibered Imager foR a Single Telescope (FIRST) is a spectro-interferometer operating in the visible wavelengths at a resolution of about 3,000. It is fed by the SCExAO system. FIRST was developed in collaboration with the Paris Observatory.

FIRST feeds a Photonic Lantern device from a focal plane. The Photonic Lantern consists of a multi-mode input slowly transitioning into 19 Single-mode fibers. The multi-mode input’s core has a diameter of 25 micrometers. 

```{image} PL_img_hardware.png
:width: 500 px
```
*Figure 1: Photonic Lantern hardware. The input is a multi-mode fiber, and the outputs are 19 single mode fibers spliced into a V-groove*


The 19 outputs of the Photonic Lantern feed a mid resolution spectrograph (R~3,000), optimized for wavelengths ranging from 600 nm to 780 nm. The spectrograph is equipped with a wollaston, allowing to split the polarization for each output, providing a total of 38 spectra (see below). More information on the instrument and its integration on SCExAO are available [here](https://arxiv.org/abs/2407.15412).

```{image} Betelgeuse_mean_img.png
```
*Figure 2: Example of imaging of `Aua (Betelgeuse) using the Photonic Lantern. This image is averaged from 200,000 frames, and displays 38 spectra, corresponding to the two polarizations from each of the 19 outputs of the Photonic Lantern.*

## General information

| FIRST parameters |  |  |
| - | - | - |
| Operating wavelength | 620 - 780 nm |  |
| Spectral resolution | R~3,000 |  |
| Spatial resolution | 25 mas |  |
| Field of view | 80 mas @ f/8 | Optimal injection efficiency is for a focal ratio of 8, providing a field of view of 80 mas.The field of view is defined as the area where the injection efficiency drops to 50% compared to the center of the field. |
| Exposure times | 7.2us - 1800 s. | Fast or Slow readout modes possible |


## Observing Modes

### Mode 1: Spectro-astrometry

**Capability:** Sub-λ/D measurement of photocenter position as a function of wavelength, enabling spatial information retrieval at scales well below the diffraction limit.

**Science applications:**<br>
-- Mapping accretion signatures on protoplanets via Hα emission
-- Detecting asymmetries in stellar environments
-- Measuring spatial distribution of spectral features

**Data requirements:**<br>
-- **Primary:** FIRST-PL camera acquisition
-- **Auxiliary (required):** Focal plane images from at least 1 of 2 additional cameras (SCExAO/VAMPIRES and/or SCExAO internal IR camera)

**On-sky calibration requirements:**<br>
-- Self-calibrating via tip-tilt telemetry from auxiliary cameras
-- No separate calibrator star observation required

**Off-sky calibration requirements:**<br>
-- Wavelength calibration (Neon lamp)
-- Flat field calibration (Halogen lamp)
-- Dark frames 

---

### Mode 2: On-Axis Imaging

**Capability:** λ/D spatial resolution within the field of view of the photonic lantern (~130 mas), with modest contrast capabilities (contrast ~10).

**Science applications:**<br>
-- Resolving stellar surfaces and features
-- Detecting close companions within the field of view
-- Characterizing compact circumstellar environments

**Data requirements:**<br>
-- **Primary:** FIRST-PL camera acquisition
-- **Auxiliary (optional):** Telemetry from SCExAO for additional wavefront/PSF monitoring

**On-sky calibration requirements:**<br>
-- Calibrator star observation (similar magnitude to target or brighter)

**Off-sky calibration requirements:**<br>
-- Wavelength calibration (Neon lamp)
-- Flat field calibration (Halogen lamp)
-- Dark frames
---

### Mode 3: Off-Axis Imaging

**Capability:** Extended field of view beyond the Photonic Lantern's intrinsic ±20 mas, enabling observations at separations up to ~1000 mas. Achieves contrast ratios >1000 at separations ≥100 mas.

**Science applications:**<br>
-- Characterizing faint companions
-- Wide binary systems
-- High contrast imaging

**Data requirements:**<br>
-- **Primary:** FIRST-PL camera acquisition
-- **Auxiliary (optional):** Telemetry from SCExAO for additional wavefront/PSF monitoring

**On-sky calibration requirements:**<br>
-- On-axis pointing on primary star (serves as calibration)
-- Interleaved on-axis/off-axis observations recommended

**Off-sky calibration requirements:**<br>
-- Wavelength calibration (Neon lamp)
-- Flat field calibration (Halogen lamp)
-- Dark frames