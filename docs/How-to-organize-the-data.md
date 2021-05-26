# Introduction
Instrument techniques and acquisition schemes work with several techniques, each delivering its own natural data structure.
In addition to this, there is also the option to work in a white beam or ToF mode.

## Data volumes
The data volumes that need to be handled vary for different operation modes. In general, a great reduction in volume can be expected from resampling the ToF spectrum. Otherwise, the data volume stays relatively high until the last steps of the analysis where model parameters are extracted.  

It should be aimed at keeping the number of files to a minimum to reduce read and write overhead of the storage and data processing. Single files per image should be avoided if possible.  On the other hand, single files per image are exactly what the current best technology is producing, so it shall be possible to handle this "worst case scenario" from the beginning, as a safety measure.

## Acquisition data structure 
Input: Datastream from detectors

Output: A single file or multiple files with raw image data possibly organized in folders depending on complexity.

# Coordinate system orientation tranformations
Provide axis transform matrix to define directions relative to for exmaple a Cathesian coordinate system oriented in the beam direction. 

## Instrument coordinate system 

## Detector plane orientation

# White beam techniques

## Radiography (__Jean__)

This mode means 1 or several images are acquired with white beam. Example of experiment when such a mode is used are
* Transmission image of an object for which no changes are measurable over the duration of the measurement
* Set of images acquired when condition of object changed over time due to internal changes in the sample or external changes applied to it (gaz pressure, quantity of water, temperature…)
* Set of images acquired to cover full sample

Data acquired that need to be connected
* Sample data sets (mandatory)
* Open beam/flat field (mandatory)
* Dark field (optional)
* Flat field with black bodies (optional)

Metadata:
* Detector type
* instrument
* Data mode (“sample”, “open beam”, “dark current”)
* Experiment proposal number
* Aperture position
* Acquisition time
* Acquisition starting time
* Motor position
* User custom metadata (defined at the DAS level) 
    * Sample name
    * type
    * size
* Diffuser type
* Diffuser thickness
* Lens used
* Scintillator thickness
* ….
* All metadata recorded by DAS automatically 

![Screen Shot 2021-04-19 at 3 03 13 PM](https://user-images.githubusercontent.com/1138324/115289646-cf11ae80-a120-11eb-9df9-4b4bc4102d1d.png)
![Screen Shot 2021-04-19 at 3 03 22 PM](https://user-images.githubusercontent.com/1138324/115289648-cfaa4500-a120-11eb-904c-a512e6b8c430.png)
![Screen Shot 2021-04-19 at 3 03 30 PM](https://user-images.githubusercontent.com/1138324/115289651-cfaa4500-a120-11eb-9682-d21fd44974e3.png)
![Screen Shot 2021-04-19 at 3 03 38 PM](https://user-images.githubusercontent.com/1138324/115289653-cfaa4500-a120-11eb-8d4f-852a4bf53464.png)
![Screen Shot 2021-04-19 at 3 03 47 PM](https://user-images.githubusercontent.com/1138324/115289654-d042db80-a120-11eb-8b17-1f78e2b3988e.png)
![Screen Shot 2021-04-19 at 3 04 02 PM](https://user-images.githubusercontent.com/1138324/115289656-d042db80-a120-11eb-9ed0-0630ea3ec13c.png)

## Tomography (__Anders__, Jean)
One file containing all projections for a single tomography. This is usually between 100-1500 frames. It is important to provide a turn-table angle per projection as a data vector (this is guaranteed in the NXTomo format).

For tomography time series there are three options depending on the acquisition scheme: 
- With step-wise turning, it is possible to produce one file per tomography. In general, it would make more sense to store all projections in a single file.
- With on-the-fly (continuous turning at constant speed) either one file with all projections can be used. Alternatively, by using a trigger signal when a turn is completed it is possible to start a new file per turn.
- Golden ratio acquisition requires all projections to be in the same file as this is an infinite series of non-overlapping projections angles.

### Reference data
The minimum of reference data for a tomography experiment is the __flat field__ image. Most detectors also require __dark current__ images, some detectors are calibrated to acquire images with zero-bias images. These images are traditional for any neutron imaging experiment and are needed for the normalization using Beer-Lambert's law :

<img src="https://latex.codecogs.com/svg.latex?I=I_0\cdot{}e^{-\int{}\mu(x)\,dx}\rightarrow%20p=-\ln\left(\frac{I}{I_0}\right)=\int{}\mu(x)\,dx" border="0"/>


In addition to the flat field and dark current images there are also some more reference images that should be stored. Examples are:
- Sample and background scattering is the origin of biases in the images. It is possible to correct using different methods, at PSI the methods described in the publications by [Boillat at al.](https://doi.org/10.1364/OE.26.015769) and [Carminari et al](https://doi.org/10.1371/journal.pone.0210300). These methods require two additional references with a black body grid inserted (with and without samples present). 
- Epithermal background for beamlines with direct w

The souce intensity is also important to consider to obtain more reliable quantification of the values in the normalized images. Therefore, is common to correct for the [_radiant exposure_](https://en.wikipedia.org/wiki/Radiant_exposure). This can be done using measurements in the images or using sensor readings:
- Radiant exposure measurements for each acquisition (1D). This would be a vector of the same character as the angle readings for the tomography. 

## Multi-modal imaging 
With multi-modal imaging one or more detectors generate data simultaneaously or in sequence. This data is in the first understanding images, but it could in priciple also be other sensors like environment data, weights, etc.

### Radiography (__Alessandro__, Anders)
 Same as for single-mode radiography, but time stamping is of particular importance. 
 Darks and flats should ideally be acquired both with and without the other radiation on and in the same configurationm as the operational one.
 In case of x-ray imaging, current and voltage of the source, use of filers or fas shutters, relative distances between source, detector and object as well as relative angle between the two radiations are essential
 The two image sizes won't necessarily match, so images should be two distinct subsets. 
 Can be operated in full simultaneity or in alternance

### Tomography (__Anders__, Alessandro)
Combination of multi-modal radiography and single mode tomograpy. Again time stamping of projections, and their acquisition angles and ionising radiation nature is crucial. 
The two volumes need to be registered (saptially aligned) a pre-alignemnt based on instrument parameters can be done but to improve further this pre-alignemnt, some techniques such as generalised DIC can be employed. The reconstructed volumes should ideally be at least pre-aligned.

## Neutron grating interferometry (__Simon__, Matteo)
The most relevant parameters of an nGI measurement are the correlation length and the orientation of the sample to the grating lines.
The correlation length depends on the wavelength, the position of the sample and the specific nGI setup.
As nGI is only sensitive to scattering perpendicular to the grating lines, an anisotropy in the scattering of the sample is determined by the relative rotation of the sample to the slits of the gratings.
For continuous reactor sources we (ANTARES) perform a full phase stepping scan for each correlation length and/or orientation of the sample.
A correlation length scan is performed by variation of the position of the sample and an anisotropy scan by simultaneous rotation of all gratings (or the sample).
It would be the task of the instrument control software to extract the necessary parameters and calculate the correlation length (be it from a change in position, wavelength or of the general setup) as well as the angle between sample and grating lines.

Every nGI measurement performs a phase scan by stepping through the period of one grating.\
Every nGI phase scan has a corresponding reference/open-beam phase scan.\
Not every phase scan has necessarily a new reference scan.\
Every single image has a corresponding offset/dark image.\
Not every image has necessarily a new offset image.

A single phase scan should always be stored in the same file with the grating position stored in a data vector.
The reference scans and the offset images should be kept separate, as they can be used for different phase scans.
However, it would be beneficial to store all data of a external parameter scan (e.g. temperature, B-field) in a single Nexus file with the option to have anisotropy and correlation length scans for each measurement.
This would then include reference scans and offset images.

Example:
```
temperature_scan/
├─ temperature1/
│  ├─ correlationlength1/
│  │  ├─ phasescan/
│  │  │  ├─ step1 (linkto_off1)
│  │  │  ├─ step2 (linkto_off1)
│  │  │  ├─ ...
│  │  │  ├─ step_values
│  │  ├─ linkto_ref1
│  ├─ correlationlength2/
│  │  ├─ anisotropy1/
│  │  │  ├─ phasescan/
│  │  │  ├─ linkto_ref1
│  │  ├─ anisotropy2/
│  │  │  ├─ phasescan/
│  │  │  ├─ linkto_ref2
│  │  ├─ ...
│  │  ├─ anisotropy_values
│  ├─ correlationlength_values
├─ temperature2/
│  ├─ phasescan/
│  │  ├─ ...
│  ├─ linkto_ref2
├─ temperature3/
│  ├─ ...
├─ temperature_values
├─ referencescans/
│  ├─ ref1/
│  │  ├─ step1 (linkto_off2)
│  │  ├─ ...
│  │  ├─ step_values
│  ├─ ref2/
│  │  ├─ ...
│  ├─ ...
├─ offsetimages/
│  ├─ off1
│  ├─ off2
│  ├─ ...

```

### Metadata
Store correlation length AND raw parameters. Is the sample in front of G1 or behind?
All grating properties and all dimensions of the setup, i.e. everything you need to understand potential errors in the measurements.
Same for the reference phase scan.

### Wavelength resolved nGI
Wavelength resolved nGI means correlation length resolved nGI.
It should therefore be possible to sort ToF binned data into the above mentioned scheme.

### Tomography with grating interferometry	
Two acquisition schemes can be used for tomography with grating interferometry:

Full CT-turn for each phase step -> possible large errors due to drift in grating positions\
Full phase scan for each step + reference scan every few steps

Any of the methods can be reordered in the scheme as described above by looking at the tomography as a scan of an external parameter.

If the filesize would be too large to handle, we can link multiple files.
In any case, the smallest subset of data would be a phase scan.

## Polarized neutron imaging (Soren, __Anders__, Alessandro)

* Beam
  * Type (Mono-chromatic / White beam / ToF)
  
* Polarizer
  * [NXPolarizer](https://manual.nexusformat.org/classes/base_classes/NXpolarizer.html#index-0)
  * ID
  * Name
  * Size (opening dimensions)
  * Position (position in lab reference frame)
  * State (ON/OFF)
	
* Spinflipper
  * [NXflipper](https://manual.nexusformat.org/classes/base_classes/NXflipper.html#nxflipper) 
  * ID
  * Name
  * Type (X, Y or Z rotation axis, can also be defined by a rotation matrix)
  * Size (opening dimensions)
  * Position (position in lab reference frame)
  * State (ON/OFF)

* Spin-rotator (90 degrees) – multiple entries; typically used before and after the sample
  * [NXflipper](https://manual.nexusformat.org/classes/base_classes/NXflipper.html#nxflipper) 
  * ID
  * Name
  * Type (X, Y or Z rotation axis, can also be defined by a rotation matrix)
  * Size (opening dimensions)
  * Position (position in lab reference frame)
  * State (ON/OFF)

* Analyzer
  * [NXPolarizer](https://manual.nexusformat.org/classes/base_classes/NXpolarizer.html#index-0)
  * ID
  * Name
  * Size (opening dimensions)
  * Position (position in lab reference frame)
  * State (ON/OFF)

* [NXTomo](https://manual.nexusformat.org/classes/applications/NXtomo.html) (This is the inner loop, to be repeated for all the spin configuations)

* Check if this info is stored 
  * Rotation stage
  * Translation stage
  * Sample environment, [NXenvironment](https://manual.nexusformat.org/classes/base_classes/NXenvironment.html#nxenvironment) 
  * Eventmode data, [NXInstrument](https://manual.nexusformat.org/classes/base_classes/NXinstrument.html#nxinstrument): [NXdetector](https://manual.nexusformat.org/classes/base_classes/NXdetector.html#nxdetector) position, [NXevent_data](https://manual.nexusformat.org/classes/base_classes/NXevent_data.html#nxevent-data): time 
  * Image_stack

### nGI with polarized neutrons

# Wavelength resolved techniques 
(Not so much here yet, sorry)
With many ToF bins, it is probably a good idea to use Folders: One folder per time step with one file the projection containing all ToF bin.
If the number of ToF bins is limited (e.g. to study a single Bragg edge): Store each time step in one file containing the ToF bins and all projections for the tomography.

References from ToF can be stored in the same manner as for white beam, but now with all ToF bin also stored as one entry per frame.

## Diffraction based (__Soren__)

Can mostly be obtained with a tomographic setup, with additional detector(s) off axis to record diffraction signals:
* [NXTomo](https://manual.nexusformat.org/classes/applications/NXtomo.html)
* nDetectors (multiple detectors possible in diffraction mode)
  * for each Detector: specs, position, rotation
  * Detectortype (Farfield, Niearfield).
  * Image_stack
  
* Check if this info is stored 
  * Rotation stage
  * Translation stage
  * Sample environment


## Resonance (__Anna__, Alex)
The basic data structure for resonances imaging (at least from Anton's MCP-Timepix detector) is the following:
* _ShutterTimes.txt -> Text file containing shutter values for the detector.
      * These values are first read in with the shutterValues.txt file and determine what the Time-of-flight values are for any images to be recorded.  
* _ShutterCounts.txt -> Total number of counts obseved over the full FOV within each shutter. 
* _spectra.txt -> A tab separated text file contianing the time-of-flight value for each image along with an integrate count over the full FOV.
* Several hundereds to thousands of fits files corresponding to the select shutter values. 

With this basic structure, the usual components of a radiography measurement are recorded (white beam, sample image, and dark fields).  All of this is dropped into a single folder. Most likely we will need to script up a sorter to back up raw data (fits & txt files) while also generating new NEXUS files. 

I think the most important thing here in terms of the data structure with a new format is having a ntuple structure where we would have an image branch and a corresponding TOF branch, which could then be looped through. 

Additionally there is definetly some meta data that I can think of that currently gets manually recorded. 
* Flight path length 
* Timepix chip temperatures
* Sample info (along with tracking number)
* proton beam current from spallation source
* There are probably other things that I'm missing. 

LANSCE Specific info: 
We perform neutron time-of-flight calibration runs with thick tungsten plates (create deep and opaque resonances). The opaque resonances also help charaterize energy dependent backgrounds. This tungsten plate goes in and out of the beam in the open beam (and sample images if you are tryign to charatize energy-dependent background frm the sample). 
* Open beam without tungsten
* Open beam with tungsten
* Sample without tungsten
* Sample with tungsten




