/**
 * Asset Items
 * Author: Rodrigo E. Principe
 * email: fitoprincipe82@gmail.com
 * license: MIT
 **/
var utils = require('users/fitoprincipe/geetools:assetmanager/utils')

var AssetItem = function(id, type, parent) {
  this.id = id
  this.type = type
  this.parent = parent
  
  this.user = utils.getRoot(this.id)
  this.folder = this.id.replace(this.user+'/', '')
  
  this.label = function() {
    return '('+type.toUpperCase()+') '+this.folder
  }
}
exports.AssetItem = AssetItem

var getItems = function(id) {
  var data = ee.data.getList({'id':id})
  var result = []
  for (var i in data) {
    var value = data[i]
    var item = new AssetItem(value.id, value.type)
    result.push({label:item.label(), value:value.id})
  }
  return result
}
exports.getItems = getItems