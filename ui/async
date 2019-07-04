/**
 * Async Widgets
 * Author: Rodrigo E. Principe
 * email: fitoprincipe82@gmail.com
 * license: MIT
 **/

var helpers = require('users/fitoprincipe/geetools:helpers_js')
 
var asyncSelect = function(options) {
  
  this.loading = 'Loading...'
  
  this.options = options
  
  this.sel = ui.Select({
    items: [],
    placeholder: this.loading,
    value: null,
    onChange: options.onChange,
    disabled: options.disabled,
    style: options.style
  })
  
  this.setItems(options.items, options.value)
  
  // inherit
  // getters
  this.items = function() {return this.sel.items()}
  this.getPlaceholder = function() {return this.sel.getPlaceholder()}
  this.getDisabled = function() {return this.sel.getDisabled()}
  this.getValue = function() {return this.sel.getValue()}
  this.style = function() {return this.sel.style()}
  // setters
  this.setDisabled = function(disabled) {this.sel.setDisabled(disabled)}
  this.setPlaceholder = function(placeholder) {this.sel.setPlaceholder(placeholder)}
  this.onChange = function(callback) {this.sel.onChange(callback)}
  this.unlisten = function(idOrType) {this.sel.unlisten(idOrType)}
}

asyncSelect.prototype.setValue = function(value, trigger) {
  try {
    this.sel.setValue(value, trigger)
  } catch (e) {
    print('value '+value+' may have not been retrieved yet or is not present in items.\nYou can parse a value in function setItems')
  }
}

asyncSelect.prototype.formatItems = function(items) {
  if (helpers.object.is(items, 'Array')) {
    return items
  } else {
    return items.map(function(it){return ee.Algorithms.String(it)})
  }
}

asyncSelect.prototype.setItems = function(items, value, trigger) {
  if (items) {
    items = this.formatItems(items)
    this.sel.setPlaceholder(this.loading)
    try {
      items.evaluate(this.evaluation(value, trigger))
    } catch (e) {
      this.sel.items().reset(items)
      this.sel.setPlaceholder('Select a value')
    }
  } else {
    this.sel.setPlaceholder('No items')
  }
}

asyncSelect.prototype.evaluation = function(value, trigger) {
  var self = this
  var wrap = function(items, error) {
    if (error === undefined) {
      self.sel.items().reset(items)
      if (items.length > 0) {
        self.sel.setPlaceholder('Select a value')
      } else {
        self.sel.setPlaceholder('No items')
      }
      if (value) {
        try {
          self.sel.setValue(value.getInfo(), trigger)
        } catch (e) {
          self.sel.setValue(value, trigger)
        }
      }
    } else {
      self.sel.setPlaceholder('Error')
      print(error)
    }
  }
  return wrap
}

asyncSelect.prototype.build = function() {
  return this.sel
}

exports.asyncSelect = asyncSelect