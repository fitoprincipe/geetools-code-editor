var helpersJS = require('users/fitoprincipe/geetools:helpers_js')
var list = require('users/fitoprincipe/geetools:_tools/list').list


/** FEATURE */
var feature = {};

// GET FEATURE PROPERTY
feature.gets = function(feature, properties) {
  var list = ee.List(properties) || feature.propertyNames()
  var newlist = list.map(function(el){
    return feature.get(el)
  })
  return newlist
}

// AGGREGATE PROPERTIES
feature.reduce = function(options) {
  var opt = options || {'reducer': 'mean',
                        'properties': ee.Number(0),
                        'name': 'reduction'}
                        
  var reduc = opt['reducer'] || 'mean'
  var prop = opt['properties'] || ee.Number(0)
  var n = opt['name'] || 'reduction'
  
  var wrap = function(feat) {
    var propnames = feat.propertyNames().remove('system:index')
    var prop2use = ee.List(ee.Algorithms.If(prop, prop, propnames))
    var properties = list.intersection(prop2use, propnames)
    var values = feature.gets(feat, properties)
    var casted = values.map(function(el){return ee.Number(el)})
    return feat.set(n, ee.Number(casted.reduce(reduc)))
  }
  
  return wrap
}

exports.feature = feature