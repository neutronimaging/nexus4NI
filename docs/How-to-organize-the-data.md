# Introduction
Instrument techniques and acquisition schemes work with several techniques, each delivering its own natural data structure.
In addition to this, there is also the option to work in a white beam or ToF mode.

## Data volumes
The data volumes that need to be handled vary for different operation modes. In general, a great reduction in volume can be expected from resampling the ToF spectrum. Otherwise, the data volume stays relatively high until the last steps of the analysis where model parameters are extracted.  

It should be aimed at keeping the number of files to a minimum to reduce read and write overhead of the storage and data processing. Single files per image should be avoided if possible.  On the other hand, single files per image are exactly what the current best technology is producing, so it shall be possible to handle this "worst case scenario" from the beginning, as a safety measure.

## Acquisition data structure 
Input: Datastream from detectors

Output: A single file or multiple files with raw image data possibly organized in folders depending on complexity.

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
- Golden ratio acquisition requires all projections to be in the same file as this is an infinite series of non-overlapping projections.

## Multi-modal imaging 
With multi-modal imaging one or more detectors generate data simultaneaously or in sequence. This data is in the first understanding images, but it could in priciple also be other sensors like environment data, weights, etc.

### Radiography (__Alessandro__, Anders)
 

### Tomography (__Anders__, Alessandro)


## Neutron grating interferometry (__Simon__, Matteo)
It is a matter of discussion how to categorize this one.
The phase steps for a frame shall be stored in a single file with the grating position stored in a data vector.
For time series it can either make sense to use a single file for all frames or to split it into one file per frame.

### Wavelength resolved nGI
For wavelength resolved nGI with few frames it better to store all phase steps and ToF bins.
Depending on the length of the time series and the number of ToF bins, there should be an option to store all data in a single file or one file per time step.

### Tomography with grating interferometry	
Two acquisition schemes can be used for tomography with grating interferometry:

- A full CT-turn for each phase step. Resulting in as many files as phase steps. This scheme is most common as it requires the least translation steps of the gratings.
- A full set of phase steps before moving to the next view angle.


## Polarized neutron imaging (__Anders__, Alessandro, Soren)

### nGI with polarized neutrons

# Wavelength resolved techniques 
(Not so much here yet, sorry)
With many ToF bins, it is probably a good idea to use Folders: One folder per time step with one file the projection containing all ToF bin.
If the number of ToF bins is limited (e.g. to study a single Bragg edge): Store each time step in one file containing the ToF bins and all projections for the tomography.

References from ToF can be stored in the same manner as for white beam, but now with all ToF bin also stored as one entry per frame.

## Diffraction based (__Soren__)

## Resonance (__Anna__, Alex)






