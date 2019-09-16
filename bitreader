var decodeKey = function(key) {
  var items = ee.String(key).split('-')
  var start = ee.String(items.get(0))
  var newkey = ee.String(ee.Algorithms.If(
    items.size().eq(1),
    start.cat('-').cat(start),
    key
    ))
  items = newkey.split('-')
  var end = ee.String(items.get(1))
  var s = ee.Number.parse(start)
  var e = ee.Number.parse(end)
  return ee.List.sequence(s, e)
}

print(decodeKey('6-5'))