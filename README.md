# SolarPanelSites
A python based, GIS data visualization project to investigate ideal solar panel site locations across the U.S.A.

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
- In essence ~> fixed position system = GHI ; dynamic/tracking system = DNI
- Because we are interested in Solar Panel technology, **we focus on GHI in this project.**

**Source**: [Kipp & Zonen](https://www.kippzonen.com/News/408/The-Difference-between-Horizontal-and-Tilted-Global-Solar-Irradiance)

---

## 📊 Datasets Used

| Dataset | Description | Link |
|--------|-------------|------|
| Land Use | Land cover raster dataset (Sentinel-2, 10m resolution) | [Land Use Dataset](https://ic.imagery1.arcgis.com/arcgis/rest/services/Sentinel2_10m_LandCover/ImageServer) |
| Substations | High voltage electric substation locations across the U.S. | [US Substations Dataset](https://543rd-gpc-hub-543rd.hub.arcgis.com/datasets/electric-substations/explore?location=32.356054%2C-98.690977%2C6.41) |
| High Powered Solar Panels | Locations of U.S. solar panels above 1 MW capacity (as of 2022) | [High Powered Solar Panel Dataset](https://energy.usgs.gov/uspvdb/) |
| GHI Raster | Global Horizontal Irradiance raster data (1998–2016 averages) | [GHI Dataset](https://www.nrel.gov/gis/solar-resource-maps) |
| DNI Raster | Direct Normal Irradiance raster data (1998–2016 averages) | [DNI Dataset](https://www.nrel.gov/gis/solar-resource-maps) |
| US Shapefile | U.S. state boundaries shapefile from TIGER/Line (2024) | [US Shapefile](https://www2.census.gov/geo/tiger/TIGER2024/STATE/) |

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

To evaluate how effectively each U.S. state is leveraging its solar potential, we compare the **estimated solar energy output** based on available solar resources (GHI) and land area, with the **actual MWh of solar energy** currently being produced in that state.

---

### ☀️ The Formula

We are interested in comparing the fields of **solar irradiation resources** and **actual solar energy production** for each U.S. state.

```math
Estimated\ PV\ Output\ (MWh) = \left( \frac{GHI_{sum} \times 365}{1000} \right) \times Efficiency
```

- `GHI_sum`: The **total sum of GHI values** across a state's area, measured in **kWh/m²/day × m²**.  
  ➤ This value is calculated using zonal statistics on a GHI raster (pixel values multiplied by pixel area).

- `Efficiency`: The **estimated photovoltaic conversion efficiency**, set to **20%** to reflect real-world system performance (losses from heat, dust, inverters, etc.).

- `365 / 1000`: Converts **daily GHI** from kWh/m²/day to **annual irradiance in MWh/m²/year**.

- **Final Output Unit**:  
  The formula returns the **maximum theoretical annual energy output in MWh/year** for that state's solar-eligible land.

---

### ⚡ Actual Solar Production (MWh)

To estimate the real-world energy production from installed solar capacity, we use:

```math
Actual\ Solar\ Production = Capacity_{MW} \times 8760 \times CapacityFactor
```

- `Capacityₘ₍MW₎`: The **total installed solar capacity** in megawatts (MW) from known utility-scale plants.  
- `8760`: Total hours in a year (365 days × 24 hours).  
- `CapacityFactor`: A real-world adjustment for weather, downtime, and non-ideal performance.  
  ➤ We use **0.25 (25%)** for single-axis tracking systems in the U.S.

- **Final Output Unit**:  
  Estimated annual production in **MWh/year**.

---

### 🔁 Utilization Ratio

To assess how effectively a state is using its available solar potential:

```math
Utilization\ Ratio = \frac{Actual\ Solar\ Production}{Estimated\ PV\ Output}
```

- **> 1.0** → State is either **importing solar energy** or using limited GHI **very efficiently**.  
- **≈ 1.0** → State is **efficiently using** its solar resource.  
- **< 1.0** → Indicates **untapped solar potential**.

---

### 📝 Note:

- All calculations assume **solar-eligible land** was properly identified using a land cover raster.
- This framework is ideal for **policy analysis, investment planning**, or **visualizing solar equity across regions**.


---

#### 🧠 Why This Matters

This formula-driven framework allows us to:
- Normalize solar performance **across regions** with different geography and solar potential  
- Identify **states with high untapped potential** for investment  
- Create a **cartogram visualization** that highlights overperformers and underutilized regions  
- Provide a **data-backed case** for targeting specific areas for solar infrastructure expansion

This approach transforms raw spatial and energy data into actionable insight.



---

## 🧠 Conclusion

This analysis reveals opportunities for more strategic investment in solar infrastructure, especially in states with abundant GHI/DNI but low solar output. Combining spatial data with machine learning provides a replicable methodology for site selection and resource optimization.

---

## Considerations

- A renewable energy transistion must be managed at a local level to ensure that voltage overloading does not occur as this can damage power grid components.
- Per landuse restrictions, such as forest/marshland/mountains, not every location is suitable for solar panel installation.

---

## 🚀 Future Work

- Incorporate real-time solar irradiance data  
- Improve model accuracy with satellite-based topography (e.g., SRTM)  
- Factor in proximity to energy demand centers
- Create Utilization Cartogram across different months

