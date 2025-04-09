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

We are interested in comparing **solar irradiation resources** to **actual solar energy production** for each U.S. state. To estimate how much energy could be generated if all solar-eligible land were utilized with photovoltaic panels, we use:

```math
Estimated\ PV\ Output\ (MWh) = GHI_{sum} \times Area_{m^2} \times 365 \times Efficiency \div 1000
```

- `GHI_sum`: The **sum of GHI values** across a state's area in **kWh/m²/day**. This is extracted using zonal statistics from the NSRDB GHI raster.  
  ➤ Raster values represent **daily average solar radiation per square meter**.

- `Areaₘ²`: The **approximate surface area** of each pixel is assumed to be **16,000,000 m²**, based on a 4 km × 4 km pixel resolution in the NSRDB raster.  
  ➤ Total area is computed by multiplying the pixel area by the number of pixels per state.

- `365`: Converts daily irradiance to annual.

- `Efficiency`: Real-world performance factor of **20%** (typical for modern photovoltaic systems, accounting for real-world losses).

- `÷ 1000`: Converts from **kWh to MWh**.

✅ **Final Output Unit**: Megawatt-hours per year (**MWh/year**)

---

### ⚡ Actual Solar Production (MWh)

To estimate real-world annual energy output based on known solar plant capacity, we use:

```math
Actual\ Solar\ Production = Capacity_{MW} \times 8760 \times CapacityFactor
```

- `Capacity_{MW}`: The **total installed solar capacity** in megawatts (MW) from known utility-scale plants.
- `8760`: Total hours in a year (365 × 24).
- `CapacityFactor`: An efficiency modifier to account for clouds, downtime, and other system losses.  
  ➤ A **25% capacity factor** is used, typical for single-axis tracking systems in the U.S.

✅ **Final Output Unit**: Megawatt-hours per year (**MWh/year**)

---

### 🔁 Utilization Ratio

To assess how effectively each state is using its solar potential:

```math
Utilization\ Ratio = \frac{Actual\ Solar\ Production}{Estimated\ PV\ Output}
```

- **> 1.0** → The state may be **importing solar energy** or using its limited solar resource **very efficiently**  
- **≈ 1.0** → The state is **efficiently utilizing** its available solar resource  
- **< 1.0** → The state has **untapped solar potential**

---

### 📝 Notes

- Calculations assume **solar-eligible land** was identified using high-resolution land cover raster data.
- The NSRDB GHI raster data provides **annual average daily GHI values (kWh/m²/day)** at approximately **4 km resolution**.
- Each pixel is assumed to represent **16 million square meters**.
- This methodology is ideal for **energy policy analysis, infrastructure planning**, and **solar equity mapping**.


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

