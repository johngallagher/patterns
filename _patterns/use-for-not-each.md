---
categories: Javascript
name: Use for instead of $.each
---

Some native JS functions are *much* faster than their JQuery counterparts. A particularly slow function is JQuery's "each".
Use a "for .. in" loop instead.


### Bad

````javascript
    var newObject = {};    

    $.each(serializedArray, function() {
      newObject[this.name] = this.value;
    });

    return newObject;
````

### Good

````javascript
    var newObject = {};

    for (var element in serializedArray) {
      newObject[this.name] = element.value;
    }

    return newObject;
````
