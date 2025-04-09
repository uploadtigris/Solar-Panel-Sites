# SolarPanelSites
A python based project to investigate ideal solar panel site locations across the U.S.A.

# ☀️ Identifying Optimal Locations for High Powered Solar Panels using GIS & Machine Learning

## 📌 Introduction

As the demand for renewable energy continues to grow, maximizing the efficiency and placement of solar energy infrastructure is more critical than ever. This project uses Geographic Information Systems (GIS), remote sensing data, and machine learning to identify the most promising locations across the United States for high-powered solar panel installation.

---

## 💡 Hypothesis

**Certain areas in the United States have high solar resource potential that is currently underutilized. By analyzing land cover, solar irradiance (GHI/DNI), and proximity to existing infrastructure, we can identify optimal future solar panel sites.**

---

## 🌞 Concept: GHI vs. DNI

${GHI} = \text{DHI} + \text{DNI} \cdot \cos(\theta)$

- **GHI**: Global Horizontal Irradiance – total solar radiation received per unit area on a horizontal surface.  
- **DHI**: Diffuse Horizontal Irradiance – scattered sunlight that reaches the surface from all directions (excluding the direct sun).  
- **DNI**: Direct Normal Irradiance – direct sunlight received per unit area, measured on a surface perpendicular to the sun’s rays.  
- **θ (theta)**: Solar zenith angle – the angle between the sun and the vertical direction.

### Why the Difference Matters
- **PV panels** are typically tilted, so using just horizontal irradiance may not reflect actual power generation.  
- Knowing **both DHI and DNI** enables accurate modeling of solar panel performance and overall solar resource estimation.

**Source**: [Kipp & Zonen](https://www.kippzonen.com/News/408/The-Difference-between-Horizontal-and-Tilted-Global-Solar-Irradiance)

---

## 📊 Datasets Used

| Dataset | Description | Link |
|--------|-------------|------|
| Land Use | Land cover raster dataset (Sentinel-2, 10m resolution) | [Link to Land Use Dataset](#) |
| Substations | High voltage substation locations | [Link to Substations Dataset](#) |
| High Powered Solar Panels | Locations of existing utility-scale solar power plants | [Link to Solar Panel Dataset](#) |
| GHI Raster | Global Horizontal Irradiance raster (monthly average) | [Link to GHI Dataset](#) |
| DNI Raster | Direct Normal Irradiance raster (monthly average) | [Link to DNI Dataset](#) |
| Power Plants | Dataset of US energy plants including MWh output | [Link to Power Plants Dataset](#) |
| US Shapefile | State boundaries shapefile | [Link to US Shapefile](#) |

---

## 🛠️ Tools and Technologies

- **Python**: Data processing and machine learning (scikit-learn, rasterio, geopandas)
- **ArcGIS Pro**: Raster and spatial analysis
- **QGIS**: Supplementary GIS visualization
- **Matplotlib / Seaborn**: Data visualization
- **NumPy / Pandas**: Data manipulation and aggregation
- **KNN Clustering**: For site selection modeling

---

## 🖼️ Visualizations

### ☀️ Solar Irradiance

- **Average Monthly DNI**  
  *![Insert Image Here](images/monthly_dni.png)*

- **Average Monthly GHI**  
  *![Insert Image Here](images/monthly_ghi.png)*

- **Mean Daily DNI**  
  *![Insert Image Here](images/daily_dni.png)*

- **Mean Daily GHI**  
  *![Insert Image Here](images/daily_ghi.png)*

---

### 📈 Solar Utilization and Infrastructure

- **Cartogram of States’ Utilization Ratio**  
  *![Insert Image Here](images/utilization_cartogram.png)*

- **High Voltage Substations vs Current High-Powered Solar Panel Sites**  
  *![Insert Image Here](images/substations_vs_panels.png)*

- **Total Solar Power Produced by State vs High Powered Solar Panel Locations**  
  *![Insert Image Here](images/solar_production_vs_sites.png)*

---

### 🗺️ Spatial & Machine Learning Insights

- **Land Use Classification Map**  
  *![Insert Image Here](images/land_use_map.png)*

- **KNN Clustering Results for Optimal Sites**  
  *![Insert Image Here](images/knn_results.png)*

---

## 📐 Utilization Ratio

### The Formula

We are interested in comparing the fields of Solar Irradiation Resources and Current MWh of Solar Power being produced for each state.

#### Estimated PV Output (MWh):

\[
Estimated\ PV\ Output = \frac{(GHI \times \frac{365}{1000}) \times Area \times Efficiency}{1000}
\]

- **Efficiency**: Estimated at 20%  
- **Area**: Total land area of suitable land in each state (based on land use raster)

#### Utilization Ratio:

\[
Utilization\ Ratio = \frac{Actual\ Solar\ Production}{Estimated\ PV\ Output}
\]

Interpretation:
- **>1** → The state may be importing energy or has very efficient use of limited GHI  
- **~1** → The state is efficiently using its solar resource  
- **<1** → There's untapped solar potential  

---

## 🧠 Conclusion

This analysis reveals opportunities for more strategic investment in solar infrastructure, especially in states with abundant GHI/DNI but low solar output. Combining spatial data with machine learning provides a replicable methodology for site selection and resource optimization.

---

## 🚀 Future Work

- Incorporate real-time solar irradiance data  
- Improve model accuracy with satellite-based topography (e.g., SRTM)  
- Factor in proximity to energy demand centers  

