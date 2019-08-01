var text = require('users/gena/packages:text')
var tools = require('users/fitoprincipe/geetools:tools')

var rgbToHex = function(R,G,B) {
  // https://campushippo.com/lessons/how-to-convert-rgb-colors-to-hexadecimal-with-javascript-78219fdb
  var rgbToHex = function (rgb) { 
    var hex = Number(rgb).toString(16);
    if (hex.length < 2) {
      hex = "0" + hex;
    }
    return hex;
  };
  
  var red = rgbToHex(R);
  var green = rgbToHex(G);
  var blue = rgbToHex(B);
  return red+green+blue;
}

var labelF = function(feat, column, scale, options) {
  var cent = feat.geometry().centroid()
  var prop = ee.Algorithms.String(feat.get(column))
  return ee.Image(text.draw(prop, cent, scale, options))
}

var label = function(fc, column, scale, options) {
  var first = labelF(ee.Feature(fc.first()), column, scale, options)
  var result = fc.iterate(function(feat, start) {
    feat = ee.Feature(feat)
    start = ee.Image(start)
    label = labelF(feat, column, scale, options)
    return tools.image.cat(start, label)
  }, first)
  
  return ee.Image(result)
}

exports.labelCollection = label
exports.labelFeature = labelF
exports.rgbToHex = rgbToHex