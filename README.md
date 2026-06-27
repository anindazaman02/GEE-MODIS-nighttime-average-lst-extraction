# Satellite Modeling of Nighttime Land Surface Temperature Using MODIS Aqua

[![Google Earth Engine](https://img.shields.io/badge/Google_Earth_Engine-JavaScript-blue?logo=googleearthengine&logoColor=white)](https://earthengine.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📌 Project Overview
This repository features a cloud-optimized Google Earth Engine (GEE) script developed to extract, map, and analyze nocturnal surface temperatures. The pipeline specifically processes observations from the MODIS Aqua satellite to map average nighttime Land Surface Temperature (LST). 

Isolating nighttime thermal data is crucial for urban climate research, as it provides the baseline empirical data required to study surface urban heat island (SUHI) intensity, diurnal temperature variations, and the thermal properties of built environments.

---

## 🛰️ Sensor Details & Pixel Math

The script utilizes the **MODIS Aqua Land Surface Temperature/Emissivity 8-Day L3 Global 1km product (`MODIS/061/MYD11A2`)**. 

Using the Aqua sensor (`MYD11A2`) instead of Terra (`MOD11A2`) is a deliberate choice for nighttime modeling because its orbital overpass happens at approximately **1:30 AM**, capturing a highly stable window of nocturnal radiative cooling. 

To convert raw sensor data into actionable climate insights, the script handles the pixel math seamlessly:
* **The Scale Conversion:** It isolates the `LST_Night_1km` band and multiplies it by the official calibration factor of $0.02$ to convert the data into Kelvin ($K$).
* **Celsius Reclassification:** It subtracts $273.15$ from the scaled pixels to output the final layers directly in **Degrees Celsius (°C)**.
* **Property Preservation:** The mapping function preserves the metadata timestamps (`system:time_start`) from the raw image stack to ensure continuous chronological tracking.

---

## 📊 Operational Steps inside the Script

1. **Asset Ingestion:** Reads the designated study area vector geometry out of your cloud asset registry (`projects/ee-zaman02/assets/Urban_Areas_2000`).
2. **Collection Filtering:** Restricts the 8-day satellite data stack to the targeted date limits and crops the bounding box to the area of interest boundary.
3. **Mean Temporal Reduction:** Collapses the multi-scene stack down into a single, cloud-free composite using a `.mean()` reduction matrix.
4. **Thermal Calibration:** Optimizes the visual limits (`min: 15` to `max: 30`) to perfectly account for cooler night environments compared to blazing daytime profiles.
5. **Zonal Statistics:** Runs a regional spatial reducer (`ee.Reducer.mean()`) to compute and print the absolute average baseline temperature of the entire zone into the console log.

---

## 💻 How to Use and Adapt This Script

### Prerequisites
* A valid [Google Earth Engine](https://earthengine.google.com/) account.
* A study area polygon or feature collection uploaded to your personal GEE asset manager.

### Execution Instructions
1. Open your workspace canvas at the online [GEE Code Editor](https://code.earthengine.google.com/).
2. Copy the code from this repository and paste it into your canvas workspace.
3. Update the asset path string in **Step 1** to match your target geography file path.
4. Click **Run**. The map frame will center automatically, rendering a smooth 1km thermal distribution matrix where dark blue zones indicate rapid heat release and bright red spots isolate nocturnal heat retention centers.

---

## 📦 Built-In Export Options (Commented Block)
The script contains pre-configured export code blocks (`Export.image.toDrive`). If un-commented, running the task will push your finalized seasonal nighttime LST layer straight to your personal Google Drive account as a georeferenced **GeoTIFF** raster. This raster maintains its original 1-kilometer spatial scale, making it ready to be dropped into desktop GIS platforms or time-series regression models.

---

## 📜 License
This processing tool is open-source and shared under the terms of the [MIT License](LICENSE).
