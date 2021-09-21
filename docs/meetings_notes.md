
## Tuesday, 21 September 2021

Agenda:

- Reminder
-   location of repository (this one)
-   [NXTomo](https://manual.nexusformat.org/classes/applications/NXtomo.html)
-   files to edit [how to organize the data](https://github.com/neutronimaging/nexus4NI/blob/main/docs/How-to-organize-the-data.md)
-   slack channel (NeXus channel, International_neutron_imaging_team workspace)
-   techniques and PI
       * Radiography - Jean
       * Tomography - Anders, Jean
       * Multi-modal imaging
           - Radiography - Alessandro, Anders
           - Tomography - Anders, Alessandro
       * Neutron grating interferometry - Simon, Matteo
       * Polarized neutron imaging - Soren, Anders, Alessandro
       * Wavelength resolved techniques
           - Diffraction based - Soren
           - Resonance - Anna, Alex

- Tasks for next meeting
    * Use *Polarized neutron imaging* as a reference to define NeXus tags for your technique (https://github.com/neutronimaging/nexus4NI/blob/main/docs/How-to-organize-the-data.md)

- NeXus and Neutron Imaging Committee

 Name | Institutions | Fields of expertise 
 --- | --- | ---
 Soren Schmidt | ESS |
 Alex Makenzie | LANL |
 Joe Parker | J-PARC |
 Simon Sebold | FRM-II |
 Anders Kaestner | PSI | Yoga
 Bill Chuirazzi | INL |
 Alessandro Tengattini | ILL |
 Anna Fedrigo | ISIS |
 Floriana Salvemini | ANSTO |
 Paul Kienzle | NIST|
 Dan Hussey | NIST |
 Gerrit Gunther | Helmholtz |
 Stephen Nneji | ISIS/Mantid |
 Dolica Akello-Egwel | ISIS/Mantid |
 Kenan Muric | ESS |
 Matteo Busi | PSI |
 Jean Bilheux | ORNL | Python programming

- Next meeting in 1 month?
   


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
