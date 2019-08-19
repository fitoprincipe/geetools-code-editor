var helpersJS = require('users/fitoprincipe/geetools:helpers_js')

/** DICTIONARIES */
var dictionary = {};

// Extract values from a list of keys
dictionary.extractList = function(dict, list) {
  var empty = ee.List([])
  list = ee.List(list)
  dict = ee.Dictionary(dict)
  var values = ee.List(list.iterate(function(el, first) {
    var f = ee.List(first)
    var cond = dict.contains(el);
    return ee.Algorithms.If(cond, f.add(dict.get(el)), f)
  }, empty))
  return values
}

exports.dictionary = dictionary