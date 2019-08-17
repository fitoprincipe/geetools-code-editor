/***
 * A Collection of Indexes
 * 
 * Author: Rodrigo E. Principe
 * email: fitoprincipe82@gmail.com
 * License: MIT
 */
 
var help = {}

var custom_ndvi = function(NIR, RED) {
 var wrap = function(img) {
   var nir = img.select([NIR])
   var red = img.select([RED])
   var ndvi = img.expression('(NIR-RED)/(NIR+RED)', {NIR: nir, RED:red}).rename('NDVI')
   return img.addBands(ndvi)
 }
 return wrap
}
exports.ndvi = custom_ndvi

var custom_nbr = function(NIR, SWIR2) {
 var wrap = function(img) {
   var nir = img.select([NIR])
   var swir2 = img.select([SWIR2])
   var index = img.expression('(NIR-SWIR2)/(NIR+SWIR2)', {NIR: nir, SWIR2:swir2}).rename('NBR')
   return img.addBands(index)
 }
 return wrap
}
exports.nbr = custom_nbr

var custom_nbr2 = function(SWIR, SWIR2) {
 var wrap = function(img) {
   var swir = img.select([SWIR])
   var swir2 = img.select([SWIR2])
   var index = img.expression('(SWIR-SWIR2)/(SWIR+SWIR2)', {SWIR: swir, SWIR2:swir2}).rename('NBR2')
   return img.addBands(index)
 }
 return wrap
}
exports.nbr2 = custom_nbr2

var custom_evi = function(NIR, RED, BLUE) {
 var wrap = function(img) {
   var nir = img.select([NIR])
   var red = img.select([RED])
   var blue = img.select([BLUE])
   var L = ee.Number(1)
   var C1 = ee.Number(6)
   var C2 = ee.Number(7.5)
   var index = img.expression('(NIR-RED)/(NIR+(C1*RED)-(C2*BLUE)+L)', 
   {NIR: nir, 
    RED: red,
    BLUE: blue,
    L: L,
    C1: C1,
    C2: C2
   }).rename('EVI')
   return img.addBands(index)
 }
 return wrap
}
exports.evi = custom_evi

var custom_savi = function(NIR, RED, L) {
  L = ee.Number(L)
  var wrap = function(img) {
    var nir = img.select([NIR])
    var red = img.select([RED])
    
    var savi = img.expression(
      '((N - R)/(N + R + l))*(1 + l)',
      {'N': nir, 'R': red, 'l': L}).rename('SAVI')
    
    return img.addBands(savi)
  }
  return wrap
}
exports.savi = custom_savi

var SAVI = {
  custom: custom_savi,
  sentinel2: function(L) {return custom_savi('B8', 'B4', L)},
  landsat8: function(L) {return custom_savi('B5', 'B4', L)},
  landsat457: function(L) {return custom_savi('B4', 'B3', L)},
  MOD09GQ: function(L) {return custom_savi('sur_refl_b02', 'sur_refl_b01', L)},
}

var NDVI = {
 custom: custom_ndvi, 
 sentinel2: custom_ndvi('B8','B4'),
 landsat8: custom_ndvi('B5', 'B4'),
 landsat457: custom_ndvi('B4', 'B3'),
 MOD09GQ: custom_ndvi('sur_refl_b02', 'sur_refl_b01')
}
var NBR = {
 custom: custom_nbr,
 sentinel2: custom_nbr('B8','B12'),
 landsat8: custom_nbr('B5', 'B7'),
 landsat457: custom_nbr('B4', 'B7')
}
var NBR2 = {
  custom: custom_nbr2,
  sentinel2: custom_nbr2('B11','B12'),
  landsat8: custom_nbr2('B6', 'B7'),
  landsat457: custom_nbr2('B5', 'B7')
}
var EVI = {
  custom: custom_evi,
  sentinel2: custom_evi('B8','B4', 'B2'),
  landsat8: custom_evi('B5', 'B4', 'B2'),
  landsat457: custom_evi('B4', 'B3', 'B1')
}

help['NDVI'] = 'NDVI["custom"] returns the function that has NIR & RED as arguments\n'
              +'NDVI["sentinel2"] returns the function for Sentinel 2'

exports.NDVI = NDVI
exports.NBR = NBR
exports.NBR2 = NBR2
exports.EVI = EVI
exports.SAVI = SAVI
exports.help = help