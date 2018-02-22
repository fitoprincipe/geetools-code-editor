# A set of tools to use in Google Earth Engine Code Editor

Google Earth Engine is a web platform for satellite image processing 
(https://earthengine.google.com/). One way to use this platform (I think it is
the most popular way) is using an online tool called *The Code Editor*, which
let the user access the platform using a scripting language (JavaScript).

## Using the tools
Once you are in the Code Editor, at top write

    var module = require('users/fitoprincipe/geetools:NAME_OF_FILE');

See **Cloud_masks** for an example.

## Cloud masks
Applying masks for clouds, shadows and snow is a very common process. This module provides some funtions to do it directly.

#### Import module

    var cloud_mask = require('users/fitoprincipe/geetools:cloud_masks');

In the `cloud_masks` module exist the following functions:

### Sentinel 2

    var s2function = cloud_mask.sentinel2

This function is made to use in collection `COPERNICUS/S2`. 
Does not use any argument, so can be used directly on a `map`
function.

### Landsat SR

    var landsatSRfunction = cloud_mask.landsatSR

This function is made to use in:
  
- `LANDSAT/LT04/C01/T1_SR`
- `LANDSAT/LT05/C01/T1_SR`
- `LANDSAT/LE07/C01/T1_SR`
- `LANDSAT/LC08/C01/T1_SR`

It can accept one argument indicating which part you want to mask out. Options
are:

- `cloud`
- `snow`
- `adjacent` (not for Landsat 8)
- `shadow`

### Test Cloud masks

There is a module to test `cloud_masks`.

    var cloud_mask = require('users/fitoprincipe/geetools:cloud_masks_test');
    cloud_mask.sentinel2()  // Test Sentinel 2
    cloud_mask.landsat4SR()  // Test Landsat 4 SR
    cloud_mask.landsat5SR()  // Test Landsat 5 SR
    cloud_mask.landsat7SR(['cloud'])  // Test only cloud of Landsat 7 SR
    cloud_mask.landsat8SR(['cloud'], 'L8 SR only cloud')  // Test only cloud of Landsat 8 SR and assign a name to the layer
