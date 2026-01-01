# Rare Earth Mineral Prospectivity Mapping using Machine Learning

Copyright (c) 2026 Shrikara Kaudambady. All rights reserved.

## 1. Introduction

This project provides a Jupyter Notebook demonstrating an end-to-end machine learning workflow for mineral prospectivity mapping, specifically tailored for discovering rare earth element (REE) deposits. Prospectivity mapping is the process of predicting the likelihood of finding mineral deposits in a given area by integrating various geoscience datasets.

This solution uses a `Random Forest` classifier, a powerful and widely-used algorithm, to identify areas with high potential for mineralization. The notebook generates synthetic geological, geophysical, and geochemical datasets to simulate a real-world scenario, making it a self-contained and educational tool.

## 2. The Solution Explained

The core of this solution is a supervised classification model that learns the relationships between various geological features and the locations of known mineral deposits.

### 2.1. Data Simulation

Since high-quality, labeled geological data is often proprietary and difficult to obtain, this notebook generates its own synthetic data layers:

*   **Geological Formation:** A raster map representing different rock types.
*   **Geophysical Anomaly:** A raster map simulating a magnetic or gravity anomaly, often associated with mineral-rich intrusions.
*   **Structural Features:** A raster map indicating the presence of geological faults, which can act as conduits for mineralizing fluids.
*   **Known Mineral Deposits:** A set of coordinates representing locations where REE deposits have been previously confirmed. These serve as the positive training examples for our model.

### 2.2. Methodology

The workflow is as follows:

1.  **Feature Engineering:** A training dataset is constructed by sampling the values from the synthetic data layers at specific locations.
    *   **Positive Samples:** Data is extracted from the locations of the known mineral deposits.
    *   **Negative Samples:** Data is extracted from random locations where no deposits are known to exist. This provides a baseline for the model.
2.  **Model Training:** A `scikit-learn` `RandomForestClassifier` is trained on the engineered feature set. The model learns the unique "signature" of a mineral deposit based on the input data layers.
3.  **Prospectivity Mapping:** The trained model is then used to predict the probability of mineralization for *every pixel* in the entire map area.
4.  **Visualization:** The final output is a **Prospectivity Map**, a heatmap where warmer colors indicate a higher predicted probability of finding rare earth minerals. This map serves as a powerful guide for exploration geologists, helping them to focus their efforts on the most promising areas.

## 3. How to Use the Notebook

### 3.1. Prerequisites

To run this notebook, you will need Python 3 and Jupyter Notebook or JupyterLab installed. You will also need to install the following Python libraries:

```bash
pip install numpy pandas scikit-learn matplotlib seaborn
```

### 3.2. Running the Notebook

1.  Clone this repository to your local machine:
    ```bash
    git clone https://github.com/shrikarak/rare-earth-prospectivity-mapping.git
    cd rare-earth-prospectivity-mapping
    ```
2.  Start the Jupyter Notebook server:
    ```bash
    jupyter notebook
    ```
3.  Open the `prospectivity_mapping.ipynb` file in your browser.
4.  You can run all cells sequentially to see the entire workflow from data generation to the final prospectivity map.

### 3.3. Interpreting the Output

The final cell of the notebook will display four maps:
*   The three input synthetic data layers (Geology, Geophysics, Structure).
*   The final Prospectivity Map.

In the Prospectivity Map, areas with yellow or red colors are the locations the model has identified as having a high potential for rare earth mineral deposits.

## 4. Deploying with Your Own Data

While this notebook uses synthetic data, it is designed to be easily adapted for real-world use cases. To use your own data, you would need to:

1.  **Prepare Your Data:** Your geological, geophysical, and geochemical data must be in a raster format (e.g., GeoTIFF). All raster files should be co-registered, meaning they have the same extent, coordinate system, and pixel size.
2.  **Load Your Data:** Replace the synthetic data generation part of the notebook with code to load your raster files. Libraries like `rasterio` or `gdal` are excellent for this purpose.
    ```python
    import rasterio

    with rasterio.open('path/to/your/geology.tif') as src:
        geology_data = src.read(1)
    ```
3.  **Provide Training Data:** You will need a list of coordinates (in the same coordinate system as your rasters) for known mineral occurrences (positive samples).
4.  **Run the Workflow:** Once your data is loaded and formatted as NumPy arrays, the rest of the notebook's workflow (feature engineering, training, and prediction) can be run with minimal changes.

This approach allows exploration companies and geologists to leverage their existing datasets to generate data-driven insights, significantly improving the efficiency and success rate of mineral exploration campaigns.
