/**** Start of imports. If edited, may not auto-convert in the playground. ****/
var l8 = ee.ImageCollection("LANDSAT/LC08/C01/T1_SR");
/***** End of imports. If edited, may not auto-convert in the playground. *****/
/* Example of a binary decision tree

This is just a 'made up' example to show the use of the function, may not make sense in
a remote sensing way

Author: Rodrigo E. Principe
mail: fitoprincipe82@gmail.com
LICENSE: MIT
*/

var dt = require('users/fitoprincipe/geetools:decision_tree')
var c = Map.getCenter()

var i = ee.Image(l8.filterBounds(c).filterMetadata('CLOUD_COVER','greater_than',50).first())
var ndvi = i.normalizedDifference(['B5', 'B4']).rename(['ndvi'])
i = i.addBands(ndvi)

var vis = {bands:['B5','B6','B4'],min:0,max:5000}

Map.addLayer(i, vis ,'original img')

// CONDITIONS
var c1 = i.select('ndvi').gt(0.2) // ndvi value greater than 0.2
var c2 = i.select('ndvi').gt(0.8) // ndvi value less than 0.8
// var c3 = i.select('B7').gt(3000) // SWIR2 value greater than 2000

// dict of conditions
var conditions = {1:c1, 2:c2, 3:i.select('ndvi').gt(0)}

/* Decision Tree
                                                                                   yes-- class1
                                                     yes-- NDVI greater than 0.8 --
                                                                                   no-- class2
                       yes-- NDVI greater than 0.2 -- 
                                                     no-- class1
NDVI greater than 0? --
                       no-- class3
 
*/

// dict of 'paths for classes'
var classes = {'class1-1': [[3,1], [1,1], [2,1]], // [cond1, true], [cond2, true], [cond3, false]
               'class1-2': [[3, 1], [1,0]],
               'class2':[[3, 1], [1,1],[2,0]],
               'class3':[[3, 0]]
              }

var dtf = dt.binary(conditions, classes)

var result = dtf(i)

var class1 = result.select('class1')
var class2 = result.select('class2')
var class3 = result.select('class3')
var noclass = result.select('dt_mask')

Map.addLayer(i.updateMask(class1), vis, 'class1')
Map.addLayer(i.updateMask(class2), vis, 'class2')
Map.addLayer(i.updateMask(class3), vis, 'class3')
Map.addLayer(i.updateMask(noclass), vis, 'Pixels not in any class')