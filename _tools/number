var helpersJS = require('users/fitoprincipe/geetools:helpers_js')

/** NUMBER */
var number = {};

// RANDOM NUMBER
number.random = function(start, end, type) {
  var s = start || 0
  var e = end || 1
  var t = type || 'float'
  
  s = ee.Number(s)
  e = ee.Number(e)
  
  var diff = e.subtract(s)
  
  var value = ee.Number(Math.random())
  
  var result = value.multiply(diff).add(s)
  
  if (t == 'float') {
    return result}
  else if (t == 'int') {
    return result.toInt()
    }
}

// TRIM DECIMALS
number.trimDecimals = function(places) {
  
  var factor = ee.Number(10).pow(ee.Number(places).toInt())
  
  var wrap = function(number) {
    var n = ee.Number(number)
    
    var floor = n.floor()
    var decimals = n.subtract(floor)
    var take = decimals.multiply(factor).toInt()
    var newdecimals = take.toFloat().divide(factor)
    return floor.add(newdecimals).toFloat()
  }
  return wrap
}

exports.number = number