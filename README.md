# Unsupervised Crop Classification Using Google Satellite Embeddings
### Cerro Gordo County, Iowa (CDL-Guided Cluster Labeling)

This repository implements an unsupervised crop classification workflow in Google Earth Engine using Google’s annual satellite embeddings. The approach trains a cluster model on one year, assigns cluster labels based on CDL majority vote, then applies the trained model to the following year for prediction and validation.

---

## Overview

The workflow includes

* Region extraction using TIGER county boundaries  
* Annual embedding mosaics (Google Satellite Embedding V1)  
* CDL cropland and cultivated masks  
* Cascade K-Means clustering  
* Cluster label assignment using CDL majority vote  
* Application to the following year  
* Validation with CDL reference  
* Confusion matrix, accuracy stats, and Pearson r  
* Visualization of predicted and reference crop maps  

The script focuses on corn and soybean classification.

---

## Data Sources

### Google Satellite Embedding V1  
**ID**: `GOOGLE/SATELLITE_EMBEDDING/V1/ANNUAL`  
Annual image embedding mosaics used as feature inputs for unsupervised clustering.

### USDA Cropland Data Layer (CDL)  
**ID**: `USDA/NASS/CDL`  
Provides cropland, cultivated mask, and detailed crop codes.  
Used to simplify crop classes into  
* 1 = Corn  
* 2 = Soybean  
* 0 = Other

### US Census TIGER  
**ID**: `TIGER/2018/Counties`  
Used to extract Cerro Gordo County (GEOID 19033).

---

## Workflow Summary

### 1. Region Extraction
Extract county geometry as the region of interest.

### 2. Embedding Loading
Fetch annual embedding mosaic for the chosen train and test years.

### 3. CDL Processing
Load CDL layers and create  
* cropland layer  
* cultivated mask  
* simplified CDL class image for corn/soy

### 4. Clustering (Training Year)
* Mask embeddings to cultivated land  
* Randomly sample pixels  
* Train cascade K-Means (`wekaCascadeKMeans`)  
* Predict cluster membership for the training year

### 5. Cluster Label Assignment
Each cluster receives a crop label based on CDL majority vote.  
A lookup table mapping cluster → corn/soy/other is generated.

### 6. Apply Clusterer to Test Year
* Apply the trained clusterer  
* Remap clusters using the same label table  
* Produce predicted crop map for the test year

### 7. Validation
* CDL is simplified for the test year  
* Predictions and CDL are aligned  
* Stratified sampling draws balanced validation points  
* Metrics computed  
  * Confusion matrix  
  * Overall accuracy  
  * Kappa  
  * Producer accuracy  
  * User accuracy  
  * Pearson r and r²

### 8. Visualization
The map shows  
* Predicted crop map  
* CDL reference map  
* A legend for corn, soy, and other classes

---

## Key Parameters

| Parameter | Description |
|----------|-------------|
| `yrTrain` | Training year |
| `yrTest`  | Test year |
| `kLow`, `kHigh` | Min and max clusters for cascade K-Means |
| `CDL_ID_CORN` | CDL corn class code (1) |
| `CDL_ID_SOY`  | CDL soy class code (5) |
| `visCrop` | Visualization palette |

---

## Outputs

The script prints

* Cluster-to-label table  
* Confusion matrix  
* Accuracy and kappa  
* Producer and user accuracy  
* Pearson correlation  
* r²  
* Predicted and reference map layers

---

## Usage Instructions

1. Open in the Earth Engine Code Editor.  
2. Paste the full script.  
3. Run.  
4. Inspect  
   * predicted crop map  
   * CDL reference map  
   * accuracy metrics  
5. Modify  
   * training/test year  
   * cluster count  
   * sampling size  
   * region  


