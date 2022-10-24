---
number: 4
title: SpatialData
pis:
  - Luca Marconato
  - Niklas Stotzem
  - Elyas Heidari
---
# Description

**Work in progress, atm the code is not fully implemented.**

This project aims to add SpatialData as an input/output format into the MCMICRO pipeline.

We worked in two repositories:
- [https://github.com/LucaMarconato/mcmicro](https://github.com/LucaMarconato/mcmicro), spatialdata branch, to extend MCMICRO to convert data to OME-NGFF
- [https://github.com/LucaMarconato/spatialdata-mcmicro](https://github.com/LucaMarconato/spatialdata-mcmicro), to create a docker container with spatialdata installed and containing the conversion scripts that will be called by MCMICRO.

The new extended MCMICRO pipeline will call the converter script at the beginning (passing the path to the raw data) and then at each pipeline step. The converted spatialdata will be put all together in a unique .zarr file (but separating samples by assigning different spatial elements to the appropriate coordinate systems.

Since the spatialdata objects are at the momemnt not used by other tools of the pipeline, the conversion can be carried on in parallel after each processing step is done.

A future more stable effort would be to allow the user to choose after which steps of the pipeline to convert to the spatial data object. For instance by specifing in the .yml config file that only "segmentation" and "quantification" are to be taken into account.

Finally, the best scenario would be to have other tools in the pipeline that takes as input some of the converted OME-NGFF files.