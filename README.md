# Park Accessibility and Neighborhood Income in Middlesex County, New Jersey

**Eunmi (Mia) Shin**  
Command-Line GIS | Rutgers University | Fall 2026

This project examines public park accessibility in Middlesex County, New Jersey. It uses Census data, public park boundaries, and walking street networks to compare park proximity, walking access, and neighborhood income.

---

## 1. Data Sources and Processing

### Data Sources

| Dataset | Source and Creator | Year / Last Update | Original Format |
|---|---|---|---|
| Census Tracts | U.S. Census Bureau, TIGER/Line | 2024 geographic boundaries | Shapefile |
| Median Household Income and Home Value | U.S. Census Bureau, American Community Survey | 2024 ACS 5-Year Estimates; released January 29, 2026 | Census API / JSON |
| Public Parks | New Jersey Department of Environmental Protection (NJDEP), Green Acres GIS | Last data edit: July 22, 2026 | ArcGIS Feature Service / GeoJSON |
| Walking Street Network | OpenStreetMap contributors, accessed through OSMnx | Continuously updated; extraction date to be confirmed | OpenStreetMap data / network graph |

The ACS estimates cover the period from 2020 to 2024.

### Data Processing

I used Python and GeoPandas to prepare and analyze the datasets.

- **Table Join:** I joined ACS income and home value data to Census Tract boundaries using GEOID.
- **Buffer and Clip:** I created 0.5-mile buffers around selected public parks, merged overlapping buffers, and clipped them to Middlesex County.
- **Spatial Overlay:** I intersected park buffers with Census Tracts and calculated the percentage of each tract's area within 0.5 mile of a park.
- **Network Analysis:** I used OSMnx to calculate shortest walking distances from Census Tract representative locations to approximate park access nodes. Walking times were estimated using a speed of 3 mph.
- **Data Classification:** I grouped Census Tracts by income level and whether their estimated walking time to a park was within 10 minutes.

### Data Quality

I identified missing ACS estimates and calculated the coefficient of variation (CV) using margins of error. Income estimates with CV ≥ 40% were marked as highly uncertain.

The park data may not include every publicly accessible park. Park access points were approximated rather than verified as actual entrances. Walking times represent estimated access from tract representative locations, not every residence.

---

## 2. Static Maps

### Map 1. Median Household Income by Census Tract

This map shows the distribution of median household income
across Census Tracts in Middlesex County.

<p align="center">
  <img src="1. Median_Household_Income_Map.png" width="700" alt="Median Household Income">
</p>

### Map 2. Public Parks and 0.5-Mile Buffers

This map shows selected public parks and their
0.5-mile straight-line buffers.

<p align="center">
  <img src="2. 0.5-Mile_Buffer_CartoDB_Map.png" width="700" alt="Public Parks and Buffers">
</p>

### Map 3. Share of Census Tract Area Within 0.5 Mile of a Park

This map shows the percentage of each Census Tract's area
located within 0.5 mile of a public park.

<p align="center">
  <img src="3. 10_Minute_Walking_Streets_CARTO_Map.png" width="700" alt="Park Proximity by Census Tract">
</p>

### Map 4. Walking Streets Within 10 Minutes of Public Parks

This map shows walking streets within 10 minutes of public parks.

<p align="center">
  <img src="4. Share of Green space_Map.png" width="700" alt="10-Minute Walking Streets">
</p>

### Map 5. Income and Public Park Accessibility

This map compares neighborhood income levels and
estimated 10-minute walking access to parks.

<p align="center">
  <img src="5. final_static_map_3_income_vs_park_access.png" width="700" alt="Income and Park Accessibility">
</p>

---

## 3. Interactive Map

The interactive map was created using GeoPandas and Folium.

Users can switch between **CARTO Positron** and **OpenStreetMap** basemaps and select the following layers:

- Median Household Income
- Median Home Value
- Public Parks
- 0.5-Mile Park Buffers
- 10-Minute and 20-Minute Walking Streets
- Estimated 20-Minute Walking Area

The default view displays **CARTO Positron, Median Household Income, and Public Parks**.

Hovering over Census Tracts displays basic information. Clicking a tract provides additional data, including income, home value, park proximity, and estimated walking time.

<p align="center">
<iframe src='final_interactive_park_map_optimized.html' width = '700' height = '700' ></iframe>
</p>

You can also explore [this map as its own web page here](final_interactive_park_map_optimized.html)

---

## 4. Data and Map Credits

- [U.S. Census Bureau — American Community Survey](https://www.census.gov/programs-surveys/acs)
- [U.S. Census Bureau — TIGER/Line Shapefiles](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html)
- [NJDEP — Open Space GIS Data](https://services1.arcgis.com/QWdNfRs7lkPq4g4Q/ArcGIS/rest/services/Open_Space/FeatureServer/66)
- [OpenStreetMap Contributors](https://www.openstreetmap.org/copyright)
- CARTO Positron Basemap

Maps created using Python, GeoPandas, Matplotlib, OSMnx, contextily, and Folium.
