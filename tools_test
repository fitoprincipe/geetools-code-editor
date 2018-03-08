var tools = require('users/fitoprincipe/geetools:tools')

var help = {};

var sum_bands = function(add_layers) {
  var add = add_layers || false;
  
  print(tools.help['sum_bands'])
  
  var ii = tools.dict2image({a:1, b:2, c:4, d:8})
  
  var sum = tools.sum_bands()(ii)
  var sum2 = tools.sum_bands('a_name')(ii)
  var sum3 = tools.sum_bands('a_plus_d', ['a', 'd'])(ii)
  var sum4 = tools.sum_bands(null, null, false)(ii)
  
  print('Original image', ii)
  print('All default', sum)
  print('Just a name', sum2)
  print('Some bands', sum3)
  print('Only sum band', sum4)
  
  if (add) {
    Map.addLayer(sum, {},'All default');
    Map.addLayer(sum2, {},'Just a name');
    Map.addLayer(sum3, {},'Some bands');
    Map.addLayer(sum4, {},'Only sum band');
  }
}

var replace_band = function(add_layers) {
  print(tools.help['replace_band']);
  
  var i = tools.dict2image({'a':1, 'b':2, 'c':3})
  var i2 = tools.dict2image({'d':4})
  
  print('replace_band', i);
  print('add_band', i2);
  
  var result = tools.replace_band(i, 'c', i2);
  print('replace band "c"', result)
  
  if (add_layers) {
    Map.addLayer(i,{},'replace_band')
    Map.addLayer(i2,{},'add_band')
    Map.addLayer(result,{},'replace band c')
  }
}

help['sum_bands'] = 'sum_bands(add_layer)\n\n'+
                    'add_layer: whether to add or not the layers to the Map'

help['replace_band'] = 'replace_band(add_layer)\n\n'+
                       'add_layer: whether to add or not the layers to the Map'
                         

exports.sum_bands = sum_bands
exports.replace_band = replace_band
exports.help = help