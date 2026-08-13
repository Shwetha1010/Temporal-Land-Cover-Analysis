# Temporal Land-Cover Analysis — One-Page Project Summary

## Objective

This project develops a temporal land-cover classification workflow for a fixed Area of Interest (AOI) in Bengaluru using Sentinel-2 Level-2A imagery from March 2021, March 2023, and March 2025. The objective is to classify major land-cover types and quantify their spatial changes over time.

## Approach

Sentinel-2 imagery was selected for the same AOI across the three target years and the same month was used to reduce seasonal effects. The imagery was reprojected to a common 10 m UTM grid to ensure spatial consistency. The Sentinel-2 Scene Classification Layer (SCL) was used for quality masking to reduce the influence of clouds and other unsuitable pixels.

Six spectral bands were used as primary inputs: Blue, Green, Red, NIR, SWIR1, and SWIR2. Additional indices — NDVI, NDWI, NDBI, SAVI, and EVI — were calculated to capture vegetation, water, built-up, and related land-cover characteristics.

ESA WorldCover 2021 was reprojected onto the Sentinel-2 grid and consolidated into five project classes: Water, Vegetation, Agricultural Land, Built-up, and Bare Land. A Random Forest classifier was selected to model the nonlinear relationships between multispectral features and land-cover classes.

To reduce spatial leakage during evaluation, the reference data was divided into spatial blocks, with four blocks reserved exclusively for validation. Training samples were capped at 2,000 pixels per class, while validation used 500 pixels per class.

## Results

The Random Forest classifier achieved **71.72% overall accuracy**, **71.72% balanced accuracy**, and **71.56% macro F1-score** on the spatial hold-out validation set. Water achieved the strongest class-wise performance, while Agricultural Land was more difficult to distinguish, primarily due to confusion with Vegetation and Built-up classes.

Feature-importance analysis identified **SWIR1, SWIR2, Red, and NDWI** among the most influential features.

The trained model was applied consistently to the 2021, 2023, and 2025 imagery. Between 2021 and 2025, the model estimated an increase of approximately **1.41 km² in vegetation** and **0.98 km² in water**, while agricultural land decreased by approximately **1.95 km²**. Built-up area remained relatively stable, changing by approximately **−0.09 km²**.

The estimated water extent showed the largest relative change, approximately **178%**. This may reflect actual variation in surface-water extent, but it was not independently verified with higher-resolution imagery and is therefore treated as an estimated result rather than a confirmed trend.

## Key Decisions and Challenges

A common 10 m spatial grid was maintained across all years to support consistent pixel-level comparison. ESA WorldCover 2021 was selected as the reference dataset because it provides 10 m global land-cover information. A spatial rather than random validation split was used to provide a more realistic assessment of generalization to geographically unseen areas.

A major preprocessing challenge involved Sentinel-2 Level-2A radiometric offsets across different processing baselines. Metadata and product XML files were inspected to determine the appropriate `BOA_ADD_OFFSET` values before calculating surface reflectance and spectral indices. This step was essential for avoiding inconsistent reflectance values and ensuring that temporal comparisons were based on correctly interpreted Sentinel-2 data.

## Limitations

The classifier was trained using 2021 reference labels and subsequently applied to 2023 and 2025 imagery. Therefore, the later-year results represent **model-derived land-cover estimates rather than independently validated ground truth**. Spectral differences between acquisition dates and confusion between spectrally similar classes, particularly Agricultural Land and Vegetation, may influence the estimated temporal changes.

## Conclusion

The project demonstrates an end-to-end remote-sensing and machine-learning workflow covering satellite data preparation, quality masking, spectral feature engineering, reference-label preparation, spatial validation, Random Forest classification, area estimation, and temporal land-cover change analysis. The workflow provides a reproducible framework for monitoring land-cover patterns over time within a fixed AOI while explicitly accounting for reference-data and radiometric-processing limitations.
