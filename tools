/*********************************
 * Some tools
 * Author: Rodrigo E. Principe
 * License: Apache 2.0
 * email: fitoprincipe82@gmail.com
 * page: https://github.com/fitoprincipe/geetools-code-editor/wiki
 **********************************/

var helpersJS = require('users/fitoprincipe/geetools:helpers_js')
var string = require('users/fitoprincipe/geetools:_tools/string').string
var date = require('users/fitoprincipe/geetools:_tools/date').date
var collection = require('users/fitoprincipe/geetools:_tools/collection').collection
var geometry = require('users/fitoprincipe/geetools:_tools/geometry').geometry
var image = require('users/fitoprincipe/geetools:_tools/image').image
var imageCollection = require('users/fitoprincipe/geetools:_tools/imagecollection').imageCollection
var map = require('users/fitoprincipe/geetools:_tools/map').map
var number = require('users/fitoprincipe/geetools:_tools/number').number
var featureCollection = require('users/fitoprincipe/geetools:_tools/featurecollection').featureCollection
var feature = require('users/fitoprincipe/geetools:_tools/feature').feature
var list = require('users/fitoprincipe/geetools:_tools/list').list
var dictionary = require('users/fitoprincipe/geetools:_tools/dictionary').dictionary

// INTERNAL
exports.get_options = helpersJS.get_options

/** MISC */

// UUID4
// from https://stackoverflow.com/questions/105034/create-guid-uuid-in-javascript#answer-21963136
var uuid4 = helpersJS.uuid4
exports.uuid4 = uuid4

// COMPUTE BITS
var computeQAbits = function(start, end, newName) {
  var pattern = ee.Number(0)
  
  start = ee.Number(start).toInt()
  end = ee.Number(end).toInt()
  newName = ee.String(newName)
  
  var seq = ee.List.sequence(start, end)
  
  var patt = seq.iterate(
    function(element, ini) {
      ini = ee.Number(ini)
      var bit = ee.Number(2).pow(ee.Number(element));
      return ini.add(bit)
    }, pattern)
    
  patt = ee.Number(patt).toInt()
  
  var wrap = function(image) {
    var good_pix = image.select([0], [newName]).bitwiseAnd(patt).rightShift(start);
    return good_pix.toInt()
  }
  return wrap
}
exports.computeQAbits = computeQAbits

exports.geometry = geometry
exports.string = string
exports.image = image
exports.map = map
exports.number = number
exports.feature = feature
exports.list = list
exports.dictionary = dictionary
exports.featureCollection = featureCollection
exports.imageCollection = imageCollection
exports.collection = collection
exports.date = date