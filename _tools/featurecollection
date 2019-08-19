var helpersJS = require('users/fitoprincipe/geetools:helpers_js')

/** FEATURECOLLECTION **/
var featureCollection = {};

// GET UNIQUE PROPERTY VALUES
featureCollection.propertyValues = function(col, property) {
  var collist = col.toList(col.size())
  var newlist = collist.map(function(feat){
    return ee.Feature(feat).get(property)
  }).removeAll([null]).removeAll([""])
  return newlist
}

featureCollection.toDictionary = function(col, options) {
  var def = {
    properties: null,
    idField: null
  }
  var opt = helpersJS.get_options(def, options)
  
  var idField = opt.idField || 'system:index'
  
  var newdict = col.iterate(function(feat, ini){
    ini = ee.Dictionary(ini)
    var f = ee.Feature(feat)
    var id = f.get(idField)
    if (opt.idField) {
      var props = f.toDictionary(opt.properties).remove([opt.idField])
    } else {
      var props = f.toDictionary(opt.properties)
    }
    return ini.set(id, props)
  }, ee.Dictionary({}))
  return newdict
}

featureCollection.enumerateProperty = function(col, name) {
  name = name || 'enumeration'
  //var collist = col.toList(col.size())
  var enumerated = collection.enumerate(col)
  
  var featlist = enumerated.map(function(l){
    l = ee.List(l)
    var index = ee.Number(l.get(0))
    var element = l.get(1)
    return ee.Feature(element).set(name, index)
  })
  return ee.FeatureCollection(featlist)
}

featureCollection.paint = function(table, propertyName, styles) {
  var results = []
  
  var keys = Object.keys(styles)
  for (var i=0; i<keys.length; i++) {
    var key = keys[i]
    var style = styles[key]
    if (key === 'default') {
      var filter = ee.Filter.inList(propertyName, keys).not()
      var filtered = table.filter(filter)
    } else if (key === 'null') {
      key = null
      var filtered = table.filterMetadata(propertyName, 'equals', key)
    } else {
      var filtered = table.filterMetadata(propertyName, 'equals', key)
    }
    
    var painted = filtered.style(style)
    results.push(painted)
  }
  var col = ee.ImageCollection.fromImages(results)
  return col.mosaic()
}

exports.featureCollection = featureCollection