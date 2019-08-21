/**
 * SCORES
 * 
 * Author: Rodrigo E. Principe
 * Licence: MIT
 * */
 
var helpers = require('users/fitoprincipe/geetools:helpers_js')
var tools = require('users/fitoprincipe/geetools:tools')

var medoid = function(collection, options) {
    // Compute a score to reflect 'how far' is from the medoid. Same params
    // as medoid() """
    
    var def = {
      bands: null,
      discard_zeros: false,
      bandname: 'sumdist',
      normalize: true
    }
    var opt = helpers.get_options(def, options)
    
    if (!opt.bands) {
      var first_image = ee.Image(collection.first())
      var bands = first_image.bandNames()
    }

    // Create a unique id property called 'enumeration'
    enumerated = tools.imagecollection.enumerateProperty(collection)
    collist = enumerated.toList(enumerated.size())

    var over_list = function(im) {
        im = ee.Image(im)
        var n = ee.Number(im.get('enumeration'))

        // Remove the current image from the collection
        var filtered = tools.list.removeIndex(collist, n)

        // Select bands for medoid
        var to_process = im.select(bands)

        var over_collist = function(img) {
            return ee.Image(img).select(bands)
        }
        var filtered = filtered.map(over_collist)

        // Compute the sum of the euclidean distance between the current image
        // and every image in the rest of the collection
        var dist = algorithms.sumDistance(
                     to_process, filtered,
                     name=bandname,
                     discard_zeros=discard_zeros)

        // Mask zero values
        if (!opt.normalize) {
            // multiply by -1 to get the lowest value in the qualityMosaic
            dist = dist.multiply(-1)
        }

        return im.addBands(dist)
    }

    var imlist = ee.List(collist.map(over_list))

    medcol = ee.ImageCollection.fromImages(imlist)

    # Normalize result to be between 0 and 1
    if normalize:
        min_sumdist = ee.Image(medcol.select(bandname).min())\
                        .rename('min_sumdist')
        max_sumdist = ee.Image(medcol.select(bandname).max()) \
                        .rename('max_sumdist')

        def to_normalize(img):
            sumdist = img.select(bandname)
            newband = ee.Image().expression(
                '1-((val-min)/(max-min))',
                {'val': sumdist,
                 'min': min_sumdist,
                 'max': max_sumdist}
            ).rename(bandname)
            return tools.image.replace(img, bandname, newband)

        medcol = medcol.map(to_normalize)

    return medcol
}