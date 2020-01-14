---
categories: Javascript
name: Never use JQuery selectors in loops
---

Codeclimate already catches repeated (uncached) element selector statements. It may seem innocuous to write just one, but a single selection written inside a loop will also kill performance.
Cache an element outside the loop first, and/or consider refactoring with native JS methods.

## Example 1 - Simple Cache Outside Loop

### Bad
````javascript
    for (var i = 0; i < elements.length; i++) {
      var $element = $(elements[i]);
      $element.html($('.map')[0].val());
    }
````

### Good
````javascript
    var $item = $('.map')[0];
    for (var i = 0; i < elements.length; i++) {
      elements[i].html($item.val());
    }
````

## Example 2 - Refactor With Additional for Loop

### Bad
````javascript
    for (var i = 0; i < tags.length; i++) {
      var tag_name = $('.marketplace-post-tags input[data-id=' + tags[i] + ']').attr('title');
      
      // rest of block emitted
    }
````

### Good
````javascript
    var tag_selectors = []
    for (var i = 0; i < tags.length; i++) {
      tag_selectors.push('.marketplace-post-tags input[data-id=' + tag_selectors[i] + ']');
    }

    for (var i = 0; i < tag_selectors.length; i++) {
      var elements = document.getElementsByClassName(tag_selectors[i]);
      for (var j = 0; j < elements.length; j++) {
        var tag_name = elements[j].getAttribute('title');

        // rest of block omitted
      }
    }
````

## Example 3 - Refactor with forEach()

### Bad
````javascript
    for (var i = 0; i < tags.length; i++) {
      var tag_name = $('.marketplace-post-tags input[data-id=' + tags[i] + ']').attr('title');

      // rest of block omitted
    }
````

### Good
````javascript
    var tag_selectors = tags.map(function(tag) {
      return '.marketplace-post-tags input[data-id=' + tag + ']';
    });
    tag_selectors.forEach(function(selector) {
      var elements = document.getElementsByClassName(selector);
        elements.forEach(function(element) {
          var tag_name = element.getAttribute('title');

          // rest of block omitted
      });
    });
````


    
