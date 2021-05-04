## Tuesday, 4 May 2021

- Soren presented his workflow document for **polarized and diffraction**
- Jean presented **radiography**
- Anders presented **tomography**

Questions:

-  We need to agree on what are all the position measured from and direction. 
    Paul: "From the nexus manual (NXtranslation): For absolute position, the origin is the scattering center (where a perfectly aligned sample would be) 
    with the z-axis pointing downstream and the y-axis pointing gravitationally up. For a relative position the NXtranslation is taken into account before 
    the NXorientation. The axes are right-handed and orthonormal."

- Reconstructed data should also be saved in a NeXus format. VGStudio, for example, can load HDF5 files. Tomoproc has a schema for reconstructed data.
  https://manual.nexusformat.org/classes/applications/NXtomoproc.html#nxtomoproc
  
- We need to present our **ideal way of reducting the data** as this may change the way we save them
