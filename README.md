# PCattleianumInMauritius
[![DOI](https://zenodo.org/badge/284477067.svg)](https://zenodo.org/badge/latestdoi/284477067)

Random Forest (RF) and Support Vector Machine (SVM) classification of invasive alien species *Psidium cattleianum* (strawberry guava) canopy cover using 
Sentinel-2 MSI composites of Black River Gorges National Park (BRGNP), Mauritius, and detection of class changes between years.

GEE repo at https://code.earthengine.google.com/?accept_repo=users/lkoerner2/PCattleianumInMaurtius

## Use
The script is intended to be used in the GEE Code Editor at https://code.earthengine.google.com/
More information on the Code Editor at https://earthengine.google.com/platform/

## Inputs
Inputs required are:
1. Shapefile of BRGNP (the area of interest) boundaries
1. Shapefile of traning data and validation data (may be already partitioned or may be input as a single file and partitioned in script).
Classes of training and validation data should be assigned numerically as follows:
    * 1 = Low/no canopy cover
    * 2 = Mixed canopy cover
    * 3 = High guava canopy cover
1. Parameters for SVM and RF classification.

## Output
The outputs of the script are:
1. RF and SVM classified Sentinel-2 composite rasters.
1. Classification error matrix.
1. Land cover (class) change raster.

## Description of processes
### 01_Preprocess_CreateComposite
Create an image collection of Sentinel-2 MSI images within the designated time window, then create a cloud-free composite 
using code adapted from Simonetti et al. (2015) by Ceccherini et al. (2017). Export composite to Assets to use in next step.

### 02_AddFeaturesAndPartition
Add GLCM (gray-level co-occurance matrix) texture features and vegetation indices (NDVI, SRI, ReNDVI, and ARI1) features to the composite raster.
Also partition ground truth data into 70% training data and 30% validation data.
Export composite and data to Assets use in next step.

### 03_01_Optimization_SplitData
Split data into five sets of 80% training and 20% validation for 5-fold cross validation in parameter optimization.

### 03_02_Optimization_RF
Use the five sets of 80% training and 20% validation, one at a time, to complete 5-fold cross validation for parameter tuning of the Random Forest classifier.

### 03_02_Optimization_SVM
Use the five sets of 80% training and 20% validation, one at a time, to complete 5-fold cross validation for parameter tuning of the Support Vector Machine classifier.

### 04_Classification
Perform SVM and RF classification of the composite and produce classified rasters and error matrices. Export classified rasters and error matrices.

### 05_LandCoverChange
Evaluate numerical class changes between different years of classified images. Export raster of class changes.

## References
Ceccherini, G., Verhegghen, A. and Dario, S. (2017). Sentinel 2 Cloud-free Mosaic [Source code]. [Online] Available at: https://code.earthengine.google.com/38b73d7a23d972a6ac5c01c677988f89 [Accessed 01 August 2018].

Simonetti, D., Simonetti, E., Szantoi, Z., Lupi, A. and Eva, H. D. (2015). First results from the phenology-based synthesis classifier using Landsat 8 imagery. IEEE Geoscience and Remote Sensing Letters, 12(7), 1496-1500.
