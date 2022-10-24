---
number: 1
title: Background subtraction
pis:
  - Krešimir Beštak
  - Victor Perez Meza

github: SchapiroLabor/Background_subtraction
---

# Description

This project is geared to add a background subtraction module into the MCMICRO pipeline. It would perform autofluorescence, pixel level subtraction on large .ome.tif images from the Lunaphore COMET platform and in the future, additional preprocessing functions may be added.

# First day of the Hackathon

## Setup
The existing Docker image for the background subtraction module was developed with MCMICRO in mind with simply defined inputs and outputs. We discussed where in the pipeline the module would fit best, and agreed it would make most sense to use it after registration and before dearray and segmentation because the expected Lunaphore COMET output images would already be registered and stitched. 

- **Input**:  
The expected inputs for the module are the path to the registered image, and the markers file used in MCMICRO. The image file is expected to be found in the "registration" folder, and the markers.csv file should contain a "background" column with values "TRUE" for the autofluorescence channels, as well as an "exposure" column specifying the exposure times when acquiring the channel which are necessary for the background subtraction calculation.

- **Output**:   
The outputs from the module are the background subtracted image (with excluded background channels), and a modified markers file which would match the background subtracted image. Both would be found in the "background" folder produced by the process.

## MCMICRO implementation
Following the instructions in the `template.nf` file, we were able to add the default options to `defaults.yml`, call the module from `main.nf`, and wrote the script `background.nf` for module execution. The `Flow.groovy` file was adapted to add a `start-at` and `stop-at` call for the module for easier testing. At the end of the day, running the module within the pipeline produced no result, but no critical errors and we left troubleshooting for the next day.

# Second day of the Hackathon

## Troubleshooting
During testing we realized the workflow for the module was being called, but the process was not being executed. The culprit was wrong indexing from the previous day in the `Flow.groovy` file. Then, we got the error that Python couldn't be found, so we made sure the Docker image works properly. Since the image was built from a Conda environment, Python was not being detected and after specifying its path within the Docker image, it worked. The last step was setting up the module outputs to be used by processes down the pipeline which was done by reassigning the path from the registered image to the background subtracted image, and the path from the markers file to the modified markers file.

## Running the pipeline
An exemplary run of the module within the pipeline is shown below:
```
nextflow pull kbestak/mcmicro
nextflow run kbestak/mcmicro --in ./Hackathon --params ./params_background.yml
```
Where the image and the markers.csv file are found in ./Hackathon/registration and ./Hackathon respectively, and the params_background.yml contains:
```
workflow:
  start-at: background
  stop-at: background
  background: true
```
