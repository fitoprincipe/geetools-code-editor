var helpersJS = require('users/fitoprincipe/geetools:helpers_js')

/** LIST */
var list = {};

// Sequence
list.sequence = function(ini, end, step) {
    var step = step || 1
    var end = ee.Number(end)
    if (step === 0){
        step = 1
    }
    var amplitude = end.subtract(ini)
    var mod = amplitude.mod(step)
    var seq = ee.List.sequence(ini, end, step)
    var condition = mod.neq(0)
    var final = ee.Algorithms.If(condition, seq.add(end), seq)
    
    return ee.List(final)
}

// Remove duplicate values
list.removeDuplicates = function(list) {
  var newlist = ee.List([]);
  list = ee.List(list)
  var wrap = function(element, init) {
    init = ee.List(init);
    var contained = init.contains(element);
    
    return ee.Algorithms.If(contained, init, init.add(element));
  };
  return ee.List(list.iterate(wrap, newlist));
}

// Intersection
list.intersection = function(list1, list2) {
  var newlist = ee.List([])
    var wrap = function(element, first) {
        first = ee.List(first)

        return ee.Algorithms.If(list2.contains(element), first.add(element), first)
    }
    return ee.List(list1.iterate(wrap, newlist))
}

// Substract two lists
list.difference = function(list1, list2) {
  return list1.removeAll(list2).add(list2.removeAll(list1)).flatten()
}

// Filter a list with a regex value
list.filterRegex = function(list, regex, flags) {
  list = ee.List(list)
  var fl = flags || null
  var i = ee.List([])
  var f = list.iterate(function(el, first){
    var f = ee.List(first)
    var e = ee.String(el);
    var matched = ee.List(e.match(regex, fl))
    return ee.Algorithms.If(matched.size().gt(0), f.add(el), f)
  }, i)
  return ee.List(f)
}

// Remove an element by its index number
list.removeIndex = function(list, index) {
  list = ee.List(list)
  index = ee.Number(index)
  var size = list.size()
  
  var allowed = function() {
    var zerof = function(list) {
      return list.slice(1, list.size())
    }
    
    var rest = function(list, index) {
      list = ee.List(list)
      index = ee.Number(index)
      var last = index.eq(list.size())
      
      var lastf = function(list) {
        return list.slice(0, list.size().subtract(1))
      }
      
      var restf = function(list, index) {
        list = ee.List(list)
        index = ee.Number(index)
        var first = list.slice(0, index)
        return first.cat(list.slice(index.add(1), list.size()))
      }
      return ee.List(ee.Algorithms.If(last, lastf(list), restf(list, index)))
    }
    
    return ee.List(ee.Algorithms.If(index, rest(list, index), zerof(list)))
  }
  
  var condition = index.gte(size).or(index.lt(0))
  
  return ee.List(ee.Algorithms.If(condition, -1, allowed()))
}

exports.list = list