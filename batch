/* 
 * Author: Rodrigo E. Principe
 * License: Apache 2.0
 * email: fitoprincipe82@gmail.com

Some batch useful tools
*/

var tools = require('users/fitoprincipe/geetools:tools')
var helpers = require('users/fitoprincipe/geetools:helpers_js')

var getRegion = function(object, bounds) {
  bounds = bounds || false
  try {
    var name = object.name()
    if (name in ['Image', 'Feature', 'ImageCollection', 'FeatureCollection']) {
      var geom = object.geometry()
    } else {
      var geom = object
    }
    if (bounds) {
      geom = geom.bounds()
    }
    return geom
  } catch(err) {
    print(err.message)
    return object
  }
}

exports.getRegion = getRegion

/*
var getRegion = function(object) {
  try {
    var name = object.name()
    if (name === 'Image' || name === 'Feature') {
      var geom = object.geometry()
    } else {
      var geom = object
    }
    var coords = geom.getInfo()['coordinates']
    return helpers.array.toString(coords)
    
  } catch(err) {
    print(err)
    return object
  }
  
}
*/

// TASK CLASS
var Task = function(taskId, config) {
  this.id = taskId
  this.config = config
}

Task.prototype.start = function() {
    ee.data.startProcessing(this.id, this.config)
}

var IMAGE_TYPES = function(img, type) {
 var types = {  "float":img.toFloat(), 
                "byte":img.toByte(), 
                "int":img.toInt(),
                "double":img.toDouble(),
                "long": img.toLong(),
                "short": img.toShort(),
                "int8": img.toInt8(),
                "int16": img.toInt16(),
                "int32": img.toInt32(),
                "int64": img.toInt64(),
                "uint8": img.toUint8(),
                "uint16": img.toUint16(),
                "uint32": img.toUint32()}
  
  return types[type]
}

var Download = {'ImageCollection': {}, 'Table': {}, 'Image':{}}

Download.ImageCollection.toAsset = function(collection, assetFolder, options) {
  var root = ee.data.getAssetRoots()[0]['id']
  var folder = assetFolder
  if (folder !== null && folder !== undefined) {
    var assetFolder = root+'/'+folder+'/'
  } else {
    var assetFolder = root+'/'
  }
  
  var defaults = {
      name: null,
      scale: 1000,
      maxPixels: 1e13,
      region: null
    }
    
  var opt = tools.get_options(defaults, options)
  var n = collection.size().getInfo();
    
  var colList = collection.toList(n);
  
  for (var i = 0; i < n; i++) {
    var img = ee.Image(colList.get(i));
    var id = img.id().getInfo() || 'image_'+i.toString();
    var region = opt.region || img.geometry().bounds().getInfo()["coordinates"];
    var assetId = assetFolder+id
    
    Export.image.toAsset({
      image: img,
      description: id,
      assetId: assetId,
      region: region,
      scale: opt.scale,
      maxPixels: opt.maxPixels})
  }
}

Download.ImageCollection.toDrive = function(collection, folder, options) {
    
    var defaults = {
      scale: 1000,
      maxPixels: 1e13,
      type: 'float',
      region: null,
      name: '{id}',
      crs: null,
      dateFormat: 'yyyy-MM-dd',
      async: false
    }
    var opt = tools.get_options(defaults, options)
    var colList = collection.toList(collection.size());
    
    var wrap = function(n, img) {
      var geom = opt.region || img.geometry()
      var region = getRegion(geom)
      var imtype = IMAGE_TYPES(img, opt.type)
      var description = helpers.string.formatTask(n)
      var params = {
        image: imtype,
        description: description,
        folder: folder,
        fileNamePrefix: n,
        region: region,
        scale: opt.scale,
        maxPixels: opt.maxPixels
      }
      if (opt.crs) {
        params.crs = opt.crs
      }
      Export.image.toDrive(params)
    }
    
    var i = 0
    while (i >= 0) {
      try {
        var img = ee.Image(colList.get(i));
        var n = tools.image.makeName(img, opt.name, opt.dateFormat).getInfo()
        wrap(n, img)
        i++
      } catch (err) {
        var msg = err.message
        if (msg.slice(0, 36) === 'List.get: List index must be between') {
          break
        } else {
          print(msg)
          break
        }
      }
    }
  }
  
Download.Table.toAsset = function(collection, options) {
  var def = {
    description: 'myExportTableTask',
    assetId: null
  }
  var opt = tools.get_options(def, options)
  
  var full_config = {
      'type': 'EXPORT_FEATURES',
      'json': collection.serialize(),
      'description': opt.description,
      'state': 'UNSUBMITTED',
  }
  full_config.assetId = opt.assetId
  var task = new Task(ee.data.newTaskId()[0], full_config)
  return task
}

Download.Table.toDrive = function(collection, options) {
  var def = {
    description: 'myExportTableTask',
    fileNamePrefix: 'myExportTableTask',
    folder: null,
    fileFormat: 'CSV'
  }
  var opt = tools.get_options(def, options)
  
  var full_config = {
      'type': 'EXPORT_FEATURES',
      'json': collection.serialize(),
      'description': opt.description,
      'state': 'UNSUBMITTED',
      'driveFileNamePrefix': opt['fileNamePrefix'],
      'driveFolder': opt['folder'],
      'fileFormat': opt['fileFormat']
  }
  var task = new Task(ee.data.newTaskId()[0], full_config)
  return task
}

Download.Image.toAsset = function(image, options) {
  var def = {
    description: 'myExportImageTask',
    assetId: null,
    region: image,
    scale: image.projection().nominalScale().getInfo()
  }
  var opt = tools.get_options(def, options)
  
  var full_config = {
      'type': 'EXPORT_IMAGE',
      'json': image.serialize(),
      'description': opt.description,
      'state': 'UNSUBMITTED',
      'region': getRegion(opt.region),
      'scale': opt.scale
  }
  full_config.assetId = opt.assetId
  var task = new Task(ee.data.newTaskId()[0], full_config)
  return task
}

Download.Image.toDrive = function(image, options) {
  var def = {
    description: 'myExportImageTask',
    fileNamePrefix: 'myExportImageTask',
    folder: null,
    fileFormat: 'GeoTIFF',
    region: image,
    scale: image.projection().nominalScale().getInfo()
  }
  var opt = tools.get_options(def, options)
  
  var full_config = {
      'type': 'EXPORT_IMAGE',
      'json': image.serialize(),
      'description': opt.description,
      'state': 'UNSUBMITTED',
      'driveFileNamePrefix': opt['fileNamePrefix'],
      'driveFolder': opt['folder'],
      'fileFormat': opt['fileFormat'],
      'scale': opt.scale,
      'region': getRegion(opt.region)
  }
  var task = new Task(ee.data.newTaskId()[0], full_config)
  return task
}
  
exports.Download = Download