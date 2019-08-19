var helpersJS = require('users/fitoprincipe/geetools:helpers_js')

/** COLLECTIONS **/
var collection = {};

collection.enumerate = function(col) {
  var collist = col.toList(col.size())
  
  // first element
  var ini = ee.Number(0)
  var first_image = ee.Image(collist.get(0))
  var first = ee.List([ini, first_image])
  
  var start = ee.List([first])
  var rest = collist.slice(1)
  
  var list = ee.List(rest.iterate(function(im, s){
    im = ee.Image(im)
    s = ee.List(s)
    var last = ee.List(s.get(-1))
    var last_index = ee.Number(last.get(0))
    var index = last_index.add(1)
    return s.add(ee.List([index, im]))
  }, start))
  
  return list
}

exports.collection = collection