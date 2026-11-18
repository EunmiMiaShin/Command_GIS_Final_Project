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

### Map 1. Median Household Income by Census Tract 2024

This map shows the spatial distribution of median household income across Census Tracts in Middlesex County, using the 2024 ACS 5-year estimates. Darker red areas represent higher household incomes, while lighter areas represent lower incomes.

The map shows that income levels vary considerably across the county, even between nearby Census Tracts. These differences provide the socioeconomic background for examining whether park accessibility varies by neighborhood income.

Missing income estimates are shown in gray, and estimates with a coefficient of variation (CV) of 40% or higher are marked with diagonal hatching to distinguish highly uncertain data.

<p align="center">
  <img src="1. Median_Household_Income_Map.png" width="700" alt="Public Parks and Buffers">
</p>

### Map 2. Public Parks and 0.5-Mile Buffers

This map shows the distribution of selected public parks and the areas located within 0.5 mile of their boundaries. Dark green polygons represent public parks, while light green areas represent their 0.5-mile buffers.

The buffers help identify areas that are geographically close to parks. The map also shows that parks are not evenly distributed throughout Middlesex County. Some areas have overlapping buffers from several parks, while others have relatively little nearby park coverage.

However, these buffers measure straight-line distance rather than actual walking distance. Roads, barriers, and park entrances may affect whether residents can easily reach a nearby park.

<p align="center">
  <img src="2. 0.5-Mile_Buffer_CartoDB_Map.png" width="700" alt="Public Parks and Buffers">
</p>

### Map 3. Share of Census Tract Area Within 0.5 Mile of a Park

This map compares park proximity at the Census Tract level. I calculated the percentage of each tract's total area that falls within a 0.5-mile buffer of a public park using spatial overlay and area calculations.

Darker green indicates a higher percentage of land within the buffer, while lighter green indicates a lower percentage. The map makes it easier to compare park proximity between Census Tracts of different sizes.

It also shows that some tracts have almost complete coverage, while others have relatively low coverage. This measure helps identify geographic differences in park proximity, but it represents the percentage of land area, not the percentage of residents with park access.

<p align="center">
  <img src="3. Share of Green space_Map.png" width="700" alt="Park Proximity by Census Tract">
</p>

### Map 4. Walking Streets Within 10 Minutes of Public Parks

This map shows streets that can be reached within an estimated 10-minute walk from selected public parks. I used the OpenStreetMap walking network through OSMnx and assumed a walking speed of 3 miles per hour.

Blue lines represent reachable street segments, while green polygons represent public parks. Unlike the 0.5-mile buffer analysis, this map considers the connections and distances along the walking street network.

The results show how street layout can influence park accessibility. Areas that appear close to parks on a straight-line map may have longer walking routes because of limited street connections or physical barriers.

The analysis uses approximate park access points, and the blue lines represent reachable streets rather than every residential location within a 10-minute walk.
<p align="center">
  <img src="4. 10_Minute_Walking_Streets_CARTO_Map.png" width="700" alt="10-Minute Walking Streets">
</p>

### Map 5. Income and Public Park Accessibility


This map combines two variables: Census Tract median household income and estimated walking time to public parks. Census Tracts were divided into lower-income and at-or-above-median-income groups, then classified by whether the estimated walking time from their representative locations was within 10 minutes.

Red represents lower-income tracts, and blue represents tracts with income at or above the study's median cutoff. Lighter shades indicate walking times of 10 minutes or less, while darker shades indicate more than 10 minutes. Gray indicates missing or highly uncertain income data or unavailable walking routes.

By combining these variables, the map allows us to compare park accessibility across different income groups and identify areas where income and walking access patterns overlap.

However, walking time is estimated from a representative location within each Census Tract. It does not mean that every resident in the same tract has the same level of access.

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
<iframe src='final_interactive_park_map_optimized.html' width = '700' height = '850' ></iframe>
</p>

You can also explore [this map as its own web page here](final_interactive_park_map_optimized.html)

---

## 4. Data and Map Credits

- U.S. Census Bureau — American Community Survey
- U.S. Census Bureau — TIGER/Line Shapefiles
- NJDEP — Open Space GIS Data
- OpenStreetMap Contributors
- CARTO Positron Basemap

Maps created using Python, GeoPandas, Matplotlib, OSMnx, contextily, and Folium.
