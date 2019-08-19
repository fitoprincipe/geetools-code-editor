var helpersJS = require('users/fitoprincipe/geetools:helpers_js')

/** GEOMETRIES */
var geometry = {};

// mask out pixels inside a geometry (return a function for mapping)
geometry.maskInside = function(image, geometry) {
  var mask = ee.Image.constant(1).clip(geometry).mask().not()
  return image.updateMask(mask)
}

// paint geometries
geometry.paint = function(geometry, options) {
  
  var def = {
    fillColor: null,
    borderColor: 'black',
    border: 2,
    idFld: null
  }
  
  var opt = helpersJS.get_options(def, options)
  
  var type = geometry.name()
  
  // build FeatureCollection depending on geometry type
  if (type === 'Geometry') {
    geometry = ee.FeatureCollection([ee.Feature(geometry)])
  } else if (type === 'Feature') {
    geometry = ee.FeatureCollection([geometry])
  }
  
  // Fill  
  if (opt.fillColor !== null) {
    var fill = ee.Image().paint(geometry, 1).visualize({palette:[opt.fillColor]})
  }
  
  // Border
  var out = ee.Image().paint(geometry, 1, opt.border).visualize({palette:[opt.borderColor]})
  
  // Fill & Border
  if ((opt.fillColor !== null) & (opt.borderColor !== null)){ var rgbVector = fill.blend(out)}
  
  // Only Fill
  else if (opt.fillColor !== null){ var rgbVector = fill } 
  
  // Only Border
  else { var rgbVector = out }
  
  // Get id image
  if (opt.idFld !== null) {
    var idImg = geometry.reduceToImage([opt.idFld], ee.Reducer.first()).rename(opt.idFld)
    return rgbVector.addBands(idImg)
  } else {
    return rgbVector
  }
}

exports.geometry = geometry