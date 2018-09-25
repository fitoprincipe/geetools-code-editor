# A set of tools to use in Google Earth Engine Code Editor

Google Earth Engine is a web platform for satellite image processing
(https://earthengine.google.com/). One way to use this platform (probably
the most popular way) is using an online tool called *The Code Editor*, which
let the user access the platform using a scripting language (JavaScript).

## READ FIRST: Using the tools
Once you are in the Code Editor, at top write

```javascript
var module = require('users/fitoprincipe/geetools:NAME_OF_FILE');
```

**Important**: Where it says `NAME_OF_FILE` replace with the name of the
module.

**Example**:

```javascript
var cld_mask_module = require('users/fitoprincipe/geetools:cloud_masks');
```

## HELP! (directly in the Code Editor)
If you need to access the help in the code editor, you can do

```javascript
var module = require('users/fitoprincipe/geetools:MODULE_NAME');
print(module.help);  // will print all help for MODULE_NAME
print(module.help[FUNCTION_NAME]);  // will print help for FUNCTION_NAME

// EXAMPLE
var tools = require('users/fitoprincipe/geetools:tools');
print(tools.help['dict2image']); // Help for dict2image function
```

To see all functions inside a module there is a special variable called
`options` that will show all options. Example:

```javascript
var tools = require('users/fitoprincipe/geetools:tools');
print(tools.options);  // Print the name of all functions inside tools
```

## Cloud masks
Applying masks for clouds, shadows and snow is a very common process. This module provides some funtions to do it directly.

#### Import module

```javascript
var cloud_masks = require('users/fitoprincipe/geetools:cloud_masks');
```

In the `cloud_masks` module exist the following functions:

### Sentinel 2

```javascript
var sentinel2function = cloud_masks.sentinel2(options)
```

This function is made to use in collection `COPERNICUS/S2`. Options are:
`cirrus` and/or `opaque`. If no argument is passed, will mask both.

### Landsat SR

```javascript
var landsatSRfunction = cloud_masks.landsatSR(options)
```

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

There is a module to test `cloud_masks`. All functions in that module use the
center of the map to filter by bounds, so if you move around you'll see
different images.

```javascript
var cloud_masks = require('users/fitoprincipe/geetools:cloud_masks_test');
cloud_masks.sentinel2()  // Test Sentinel 2
cloud_masks.landsat4SR()  // Test Landsat 4 SR
cloud_masks.landsat5SR()  // Test Landsat 5 SR
cloud_masks.landsat7SR(['cloud'])  // Test only cloud of Landsat 7 SR
cloud_masks.landsat8SR(['cloud'], 'L8 SR only cloud')  // Test only cloud of Landsat 8 SR and assign a name to the layer
```

## EXAMPLES

### Apply cloud mask in one Sentinel 2 Image

```javascript
var cloud_masks = require('users/fitoprincipe/geetools:cloud_masks');
var sentinel2function = cloud_masks.sentinel2();

// The original Image
var S2Image = ee.Image('COPERNICUS/S2/20171117T142739_20171117T143844_T18GYU');

// Apply mask
var masked_image = sentinel2function(S2Image);

// Visualize
var vis = {bands:['B8', 'B12', 'B4'], min:0, max:5000};
Map.addLayer(masked_image, vis, 'Sentinel 2 masked');
Map.centerObject(masked_image)
```

### Apply cloud mask in one Landsat SR Image

```javascript
var cloud_masks = require('users/fitoprincipe/geetools:cloud_masks');
var landsatSRfunction = cloud_masks.landsatSR;

// The original Image
var L8Image = ee.Image('LANDSAT/LC08/C01/T1_SR/LC08_232089_20170416');

// Apply mask
var masked_image = landsatSRfunction()(L8Image);

// Visualize
var vis = {bands:['B5', 'B7', 'B4'], min:0, max:5000}
Map.addLayer(masked_image, vis, 'Landsat 8 SR masked');
Map.centerObject(masked_image)
```
