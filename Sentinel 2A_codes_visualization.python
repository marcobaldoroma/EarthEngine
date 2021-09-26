###############################################            https://developers.google.com/earth-engine/datasets/   #################################################



Python codes to download or run the Sentinel 2 Products and many others

Follow the links and tips the python codes in Google Earth Engine (GEE)

Sentinel-2 MSI: MultiSpectral Instrument, Level-2A
COPERNICUS/S2_SR

Dataset Availability
    2017-03-28T00:00:00Z - 2021-09-24T00:00:00
Dataset Provider
    European Union/ESA/Copernicus 
Earth Engine Snippet
    ee.ImageCollection("COPERNICUS/S2_SR") open_in_new 

###############################  Sentinel-2 is a wide-swath, high-resolution, multi-spectral imaging mission supporting Copernicus Land Monitoring studies, including the monitoring of vegetation, soil and water cover, as well as observation of inland waterways and coastal areas.

The Sentinel-2 L2 data are downloaded from scihub. They were computed by running sen2cor. WARNING: ESA did not produce L2 data for all L1 assets, and earlier L2 coverage is not global.......
______________________________________________________________________________________________
/**
 * Function to mask clouds using the Sentinel-2 QA band
 * @param {ee.Image} image Sentinel-2 image
 * @return {ee.Image} cloud masked Sentinel-2 image
 */
function maskS2clouds(image) {
  var qa = image.select('QA60');

  // Bits 10 and 11 are clouds and cirrus, respectively.
  var cloudBitMask = 1 << 10;
  var cirrusBitMask = 1 << 11;

  // Both flags should be set to zero, indicating clear conditions.
  var mask = qa.bitwiseAnd(cloudBitMask).eq(0)
      .and(qa.bitwiseAnd(cirrusBitMask).eq(0));

  return image.updateMask(mask).divide(10000);
}

var dataset = ee.ImageCollection('COPERNICUS/S2_SR')
                  .filterDate('2020-01-01', '2020-01-30')
                  // Pre-filter to get less cloudy granules.
                  .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE',20))
                  .map(maskS2clouds);

var visualization = {
  min: 0.0,
  max: 0.3,
  bands: ['B4', 'B3', 'B2'],
};

Map.setCenter(83.277, 17.7009, 12);

Map.addLayer(dataset.mean(), visualization, 'RGB');

______________________________________________________________________________________________
________________________ View of the night sky, focused on Lights in Europe in 1990  at 500m resolution (BRIGHTNESS)___________________

________________________________________________________________________________________________________________________
// Compute the trend of nighttime lights from DMSP.

// Add a band containing image date as years since 1990.
function createTimeBand(img) {
  var year = img.date().difference(ee.Date('1990-01-01'), 'year');
  return ee.Image(year).float().addBands(img);
}

// Fit a linear trend to the nighttime lights collection.
var collection = ee.ImageCollection('NOAA/DMSP-OLS/CALIBRATED_LIGHTS_V4')
    .select('avg_vis')
    .map(createTimeBand);
var fit = collection.reduce(ee.Reducer.linearFit());

// Display a single image
Map.addLayer(ee.Image(collection.select('avg_vis').first()),
         {min: 0, max: 63},
         'stable lights first asset');

// Display trend in red/blue, brightness in green.
Map.setCenter(30, 45, 4);
Map.addLayer(fit,
         {min: 0, max: [0.18, 20, -0.18], bands: ['scale', 'offset', 'scale']},
         'stable lights trend');

__________________________________      Python script to insert in GEE for Sentinel 2A Ortophotos view  10m hyper-resolution       ____________________________
#####   This data from Planet labs Inc. SkySat satellites was collected for the experimental "Skybox for Good Beta" program in 2015, 
#####   as well as for various crisis response events and a few other projects. The data is available in both a 5-band Multispectral/Pan collection, and a …

Link::  https://developers.google.com/earth-engine/datasets/tags/highres

________________________________________________________________________________________________________________________
var dataset = ee.ImageCollection('SKYSAT/GEN-A/PUBLIC/ORTHO/RGB');
var rgb = dataset.select(['R', 'G', 'B']);
var rgbVis = {
  min: 11.0,
  max: 190.0,
};
Map.setCenter(-70.892, 41.6555, 15);
Map.addLayer(rgb, rgbVis, 'RGB');

___________________________________- Phyton Script for Agricultural land in USA 2003 high-resulution______________-

The National Agriculture Imagery Program (NAIP) acquires aerial imagery during the agricultural growing seasons in the continental U.S.

NAIP projects are contracted each year based upon available funding and the imagery acquisition cycle. Beginning in 2003, NAIP was acquired on a 5-year cycle. 2008 was a transition year, and a three-year cycle began in 2009.

NAIP imagery is acquired at a one-meter ground sample distance (GSD) with a horizontal accuracy that matches within six meters of photo-identifiable ground control points, which are used during image inspection.

Older images were collected using 3 bands (Red, Green, and Blue: RGB), but newer imagery is usually collected with an additional near-infrared band (RGBN). RGB asset ids begin with 'n', NRG asset ids begin with 'c', RGBN asset ids begin with 'm_'.

________________________________________________________________________________________________________________________
var dataset = ee.ImageCollection('USDA/NAIP/DOQQ')
                  .filter(ee.Filter.date('2017-01-01', '2018-12-31'));
var trueColor = dataset.select(['R', 'G', 'B']);
var trueColorVis = {
  min: 0.0,
  max: 255.0,
};
Map.setCenter(-73.9958, 40.7278, 15);
Map.addLayer(trueColor, trueColorVis, 'True Color');

_______________________________________________ Planet SkySat Public Ortho Imagery, Multispectral _________________________
SKYSAT/GEN-A/PUBLIC/ORTHO/MULTISPECTRAL

Dataset Availability
    2014-07-03T00:00:00Z - 2016-12-24T00:00:00
Dataset Provider
    Planet Labs Inc. 
Earth Engine Snippet
    ee.ImageCollection("SKYSAT/GEN-A/PUBLIC/ORTHO/MULTISPECTRAL") open_in_new 

#######################   DETAILS   ###########################

NAIP: National Agriculture Imagery Program
USDA/NAIP/DOQQ

Dataset Availability
    2003-01-01T00:00:00Z - 2019-01-01T00:00:00
Dataset Provider
    USDA Farm Production and Conservation - Business Center, Geospatial Enterprise Operations 
Earth Engine Snippet
    ee.ImageCollection("USDA/NAIP/DOQQ")
    
 
 ########### This data from Planet labs Inc. SkySat satellites was collected for the experimental "Skybox for Good Beta" program in 2015, as well as for various crisis response events and a few other projects. The data is available in both a 5-band Multispectral/Pan collection, and a Pansharpened RGB collection.

Each image's asset ID contains the acquisition date and time, for example, image s01_20150304T080608Z was acquired on March 4, 2015 at 08:06 Zulu (UTC). For more information, please see the Planet Imagery Product Specifications and visit the Planet Imagery and Archive site.

This Multispectral/Pan collection contains images with five 16-bit bands shifted up from the original 12-bit data. The B, G, R, and Near-IR bands have a resolution of approximately 2m per pixel, while the Pan band is approximately 0.8m resolution (closer to 1m for off-nadir images).
Explore in Earth Engine  

___________________________________________________________________________________

var dataset = ee.ImageCollection('SKYSAT/GEN-A/PUBLIC/ORTHO/MULTISPECTRAL');
var falseColor = dataset.select(['N', 'G', 'B']);
var falseColorVis = {
  min: 200.0,
  max: 6000.0,
};
Map.setCenter(-70.892, 41.6555, 15);
Map.addLayer(falseColor, falseColorVis, 'False Color');


 
 
