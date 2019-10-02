# Detach DOM elements before manipulation

DOM manipulation is always slow. It is better to avoid manipulating directly onto the live DOM if at all possible.
Simply removing with `detach()`, manipulating and re-adding will improve performance. Be careful to re-attach the element in the same position!

### Bad
````javascript
  var showFeatured = function(points, data) {
    var $popup = $('<div class="component-map-popup"><strong>' + data.title + '</strong><br />' +
                   data.location + '</div>');
    var $map = $('#map');
    $popup.css({
      position: 'absolute',
      left: (points.x + 10) + 'px',
      top: (points.y + 10) + 'px'
    });

    $map.append($popup);
  }
````

### Good
````javascript
  var showFeatured = function(points, data) {
    var $popup = $('<div class="component-map-popup"><strong>' + data.title + '</strong><br />' +
                   data.location + '</div>');
    var $map = $('#map');
    $popup.css({
      position: 'absolute',
      left: (points.x + 10) + 'px',
      top: (points.y + 10) + 'px',
    });

    $map.detach();
    $map.append($popup);

    // Take care to use the correct insertion function
    $parent.append($map);
  }
````