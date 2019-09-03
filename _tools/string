var helpersJS = require('users/fitoprincipe/geetools:helpers_js')

/** STRINGS */
var string = {};

// format
string.format = function(string, replacement) {
  var str = ee.String(string)
  var repl = ee.Dictionary(replacement)
  var keys = repl.keys()
  var values = repl.values().map(function(v){return ee.Algorithms.String(v)})
  var z = keys.zip(values)
  
  var newstr = z.iterate(function(kv, ini){
    var keyval = ee.List(kv)
    ini = ee.String(ini)
    
    var key = ee.String(keyval.get(0))
    var value = ee.String(keyval.get(1))
    
    var pattern = ee.String('{').cat(key).cat(ee.String('}'))
    
    return ini.replace(pattern, value)
    
  }, str)
  return ee.String(newstr)
}

exports.string = string
