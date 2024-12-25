# SO2-Hotspots-in-High-Population-Regions-A-GEE-Analysis

![Screenshot (45)](https://github.com/user-attachments/assets/7fda7acb-dda7-4b6b-bd5e-f96363778362)


Loading the Boundary for Nairobi:

The FAO/GAUL/2015/level2 dataset provides administrative boundaries.
Filters are applied to select the boundary for Nairobi using:
javascript
Copy code
ee.Filter.eq('ADM1_NAME', 'Nairobi') // County name
ee.Filter.eq('ADM2_NAME', 'Nairobi') // District name
This ensures the analysis is confined to Nairobi's administrative region.
Fetching Sentinel-5P SO2 Data:

COPERNICUS/S5P/OFFL/L3_SO2 is the dataset used to measure sulfur dioxide (SO2) levels in the atmosphere.
The script selects the SO2_column_number_density band, representing SO2 concentration in the air.
Data is filtered for specific date ranges across four years: 2019, 2022, 2023, and 2024.
These filtered datasets are merged into a single collection and further restricted to Nairobi's boundary using:
javascript
Copy code
.filterBounds(nairobi)
Clipping the Data:

The SO2 data is clipped to Nairobi's boundary using:
javascript
Copy code
collection.map(function(image) {
  return image.clip(nairobi);
});
This ensures the visualization is strictly limited to Nairobi's region.
Visualizing SO2 Data:

A color palette is defined to represent different SO2 concentration levels:
javascript
Copy code
palette: ['black', 'blue', 'purple', 'cyan', 'green', 'yellow', 'red']
Lower concentrations are black/blue, while higher concentrations are red.
Loading and Clipping Population Data:

The WorldPop/GP/100m/pop dataset provides global population estimates.
The script filters the dataset to include only data within Nairobi's boundary and calculates the average population:
javascript
Copy code
var clippedPopulation = population.mean().clip(nairobi);
Visualization parameters are defined with a color palette representing population density levels.
Adding Layers to the Map:

Two layers are added:
SO2 concentrations ('S5P SO2 - Nairobi').
Population density ('Population - Nairobi').
These layers allow for a visual comparison between air quality and population density in Nairobi.
Setting the Map’s Center:

The map is centered on Nairobi's coordinates (36.8219, -1.2921) with a zoom level of 10.
Output:
SO2 Layer: Shows the distribution of sulfur dioxide over Nairobi for the selected periods.
Population Layer: Visualizes population density across Nairobi.
Map Center: Automatically zooms into Nairobi for a clear view of both datasets.
