# Ember Timetree

Visualize hierarchical timeline data. Built with [Ember.js](http://emberjs.com) and [D3.js](http://d3js.org).

<a href="http://CrowdStrike.github.io/ember-timetree"><img src="examples/screenshot_timetree.png" alt="timetree example" title="Peep the demo" align="middle"/></a>

Peep [the demo](http://CrowdStrike.github.io/ember-timetree).

## Basic Usage

Include the following on your page:

1. [Ember](http://emberjs.com)
2. [D3](http://d3js.org)
3. `ember-timetree.js`
4. Styles to get you started: <a href="examples/css/timetree_basic.css"><code>timetree_basic.css</code></a> (SVG default styles don't work so well with Timetree)

An example of the simplest view in your Handlebars,

```handlebars
{{view Ember.Timetree.TimetreeView contentBinding="YourApp.YourTimetreeArray"}}
```

where `YourTimetreeArray` is an array of objects representing **the rows** of the timetree.

## Row Object

Each row object is a plain JavaScript object defining at least a display name, a start time, and an end time. Here is the full set of fields.

```javascript
{
  /* REQUIRED FIELDS */

  label:     "Name",
  start:     12345,      // Milliseconds since the UTC epoch.
  end:       67890,

  /* OPTIONAL FIELDS */

  parent:    3,          // Index of this row's hierarchical parent in the array.
  id:        "123456L",  // Id for determining uniqueness; defaults to index in the array.
  className: "info",     // CSS class name for this row's labels and bars.

  content:   {},         // Arbitrary content to bind when the user selects (clicks on) a
                         // row. Useful if you want to do exact identity comparison to the
                         // selection. If empty, selecting a row binds `content` to the
                         // row object itself (which ember-timetree may have transformed,
                         // so don't count on it being identical to your original input).

  sections:  [{ start: 12345, end: 23456, className: "active"   },  // Start/stop this row's timeline multiple times.
              { start: 23456, end: 67890, className: "inactive" }]  // Each section can have its own, optional CSS class name.
                                                                    // Note the row object's overall start/end fields must
                                                                    // still be specified above, as its bar will still be drawn.
}
```

## More Options

### Selection

To listen for timetree clicks, set the `selectionBinding` attribute on the view. Upon the user selecting a row, the binding will contain the selected row's `content` field, or the row object itself if `content` is empty.

ember-timetree won't transform the `content` field but it may transform the row object, so don't count on the latter being identical to your original input.

### Resize on Collapse

If you set the 'resizeOnCollapse' attribute to true on the view, the height of the tree will resize when collapsing a node. This is nice when you have really long tree and you do not want white space when a node is collapsed.

### Brush View

Want to zoom and drag to focus anywhere on your timeline? After the main view, add a `Ember.Timetree.TimetreeBrushView`, and link the two via the `rangeBinding` and `brushRangeBinding` attributes, respectively.

![brush view](examples/screenshot_brush.png "brush view")

### Extending

Many methods on `TimetreeView` are ripe for extending. For example, to override the built-in date/time format:

```javascript
App.MyTimetreeView = Ember.Timetree.TimetreeView.extend({
  timeFormat: Ember.computed(function() {
    return d3.time.format.utc("your D3 date format here");
  }).property()
});
```

### Default View Options

```javascript
width:          750,
rowHeight:      15,
rowSpacing:     10,
labelsWidth:    200,
axisHeight:     20,
axisPosition:   'bottom',
indentSize:     20,
labelAlign:     'left',
contentMargin:  null,      // e.g. { top: 0, left: 0, bottom: 0, right: 0 },

collapsable:    true,      // can collapse hierarchy items?
resizeOnCollapse: false,   // resize the height of the tree when collapsing a node
scrubbable:     true,      // draw the scrubber (on hover)?
selectable:     true,      // can select rows (on click)?
brushable:      false,     // can drag-click and drag to zoom?

showLabels:     true,
showLinks:      true,

content:        null,      // bind this to the array of row objects
selection:      null,      // bind this to the selected row
brushRange:     null,      // bind this to a TimetreeBrushView
```

View-source on [the demo page](http://CrowdStrike.github.io/ember-timetree) to get more ideas how to tweak ember-timetree to your liking.

## Building

Same instructions as Ember itself\*.

1. Run `bundle install` to fetch the necessary Ruby gems.
2. Run `rake dist` to build into the `dist/` directory.

To run the tests, `rake test`.

\*See also [ember-dev](https://github.com/emberjs/ember-dev) for more on best practices for Ember packaging.

## Credits

<a href="http://crowdstrike.com"><img src="examples/CrowdStrike_logo.png" alt="CrowdStrike logo" title="Crafted By CrowdStrike" width="240" height="42"/></a>
