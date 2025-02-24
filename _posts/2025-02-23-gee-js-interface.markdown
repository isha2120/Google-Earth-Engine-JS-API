---
layout: post
title: "Interactive Map using Google Earth Engine"
date: 2025-02-23 22:14:47 +0530
categories: [Google Earth Engine, Remote Sensing, GIS]
---

## Introduction

In this blog post, we explore the Google Earth Engine JavaScript API to visualize surface temperature and elevation over India. The interactive map below allows users to explore these datasets dynamically.

## Interactive Map

Below is an interactive Google Earth Engine map embedded in this post. You can toggle between layers and explore the dataset.

<iframe src="https://isha.users.earthengine.app/view/my-app-1" width="100%" height="745px" style="border:none;"></iframe>

## Data Sources

We use the following datasets:

1. SRTM Digital Elevation Model (DEM) - Provides elevation data.
2. MODIS Land Surface Temperature (LST) - Captures surface temperature from April to June 2023.

## Code Implementation

The following code was written in Google Earth Engine JavaScript API to process and visualize the data.

```javascript
// 1. Define India's geometry using FAO country boundaries
var countries = ee.FeatureCollection('FAO/GAUL/2015/level0');
var india = countries.filter(ee.Filter.eq('ADM0_NAME', 'India')).geometry();
Map.centerObject(india, 5);

// 2. Terrain analysis using SRTM DEM
var dem = ee.Image("USGS/SRTMGL1_003").clip(india);

// 3. Surface Temperature using MODIS LST
var lst = ee.ImageCollection('MODIS/061/MOD11A2')
  .filterBounds(india)
  .filterDate('2023-04-01', '2023-06-30')
  .select('LST_Day_1km')
  .mean()
  .multiply(0.02)
  .subtract(273.15)
  .rename('LST')
  .clip(india);

// 4. Visualization parameters
var demParams = {min: 0, max: 4000, palette: ['#006633', '#E5FFCC', '#663300']};
var lstParams = {min: 20, max: 53, palette: ['blue', 'green', 'yellow', 'red']};

// 5. Add layers to the map
Map.addLayer(dem, demParams, 'Elevation (m)');
Map.addLayer(lst, lstParams, 'Surface Temperature (°C)');

// 6. Calculate Maximum Surface Temperature
var maxTemp = lst.reduceRegion({
  reducer: ee.Reducer.max(),
  geometry: india,
  scale: 1000,
  maxPixels: 1e13
});

// 7. Print Maximum Temperature
print('Maximum Surface Temperature (°C) from April to June 2023:', maxTemp.get('LST'));
 ---
layout: post
title: "Interactive Map using Google Earth Engine"
date: 2025-02-23 22:14:47 +0530
categories: [Google Earth Engine, Remote Sensing, GIS]
---

## Introduction

In this blog post, we explore the Google Earth Engine JavaScript API to visualize surface temperature and elevation over India. The interactive map below allows users to explore these datasets dynamically.

## Interactive Map

Below is an interactive Google Earth Engine map embedded in this post. You can toggle between layers and explore the dataset.

<iframe src="https://isha.users.earthengine.app/view/my-app-1" width="100%" height="775px" style="border:none;"></iframe>

## Data Sources

We use the following datasets:

1. SRTM Digital Elevation Model (DEM) - Provides elevation data.
2. MODIS Land Surface Temperature (LST) - Captures surface temperature from April to June 2023.

## Code Implementation

The following code was written in Google Earth Engine JavaScript API to process and visualize the data.

javascript
// 1. Define India's geometry using FAO country boundaries
var countries = ee.FeatureCollection('FAO/GAUL/2015/level0');
var india = countries.filter(ee.Filter.eq('ADM0_NAME', 'India')).geometry();
Map.centerObject(india, 5);

// 2. Terrain analysis using SRTM DEM
var dem = ee.Image("USGS/SRTMGL1_003").clip(india);

// 3. Surface Temperature using MODIS LST
var lst = ee.ImageCollection('MODIS/061/MOD11A2')
  .filterBounds(india)
  .filterDate('2023-04-01', '2023-06-30')
  .select('LST_Day_1km')
  .mean()
  .multiply(0.02)
  .subtract(273.15)
  .rename('LST')
  .clip(india);

// 4. Visualization parameters
var demParams = {min: 0, max: 4000, palette: ['#006633', '#E5FFCC', '#663300']};
var lstParams = {min: 20, max: 53, palette: ['blue', 'green', 'yellow', 'red']};

// 5. Add layers to the map
Map.addLayer(dem, demParams, 'Elevation (m)');
Map.addLayer(lst, lstParams, 'Surface Temperature (°C)');

// 6. Calculate Maximum Surface Temperature
var maxTemp = lst.reduceRegion({
  reducer: ee.Reducer.max(),
  geometry: india,
  scale: 1000,
  maxPixels: 1e13
});

// 7. Print Maximum Temperature
print('Maximum Surface Temperature (°C) from April to June 2023:', maxTemp.get('LST'));
```