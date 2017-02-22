[![npm](http://nagarajanchinnasamy.com/subtotal/images/subtotal_npm.svg)](https://www.npmjs.com/package/subtotal) [![tests](http://nagarajanchinnasamy.com/subtotal/images/subtotal_tests.svg)](http://nagarajanchinnasamy.com/subtotal/tests/) [![license](http://nagarajanchinnasamy.com/subtotal/images/subtotal_license.svg)](https://github.com/nagarajanchinnasamy/pivottable-subtotal-renderer/blob/master/LICENSE)


# Subtotal.js

[Subtotal.js](http://nagarajanchinnasamy.com/subtotal) is an open-source JavaScript plugin for **PivotTable.js** . Subtotal.js renders rows and columns of a pivot table with subtotals and lets the user to expand or collapse rows and columns. Its originally written by [Nagarajan Chinnasamy](https://github.com/nagarajanchinnasamy/) at [Mindtree](http://mindtree.com/).


It is available under an MIT license from [NPM](https://www.npmjs.com/package/subtotal) and [Bower](http://bower.io/) under the name `subtotal`.


[PivotTable.js](http://nicolas.kruchten.com/pivottable) is a Javascript Pivot Table library with drag'n'drop functionality built on top of jQuery/jQueryUI and originally written in CoffeeScript by [Nicolas Kruchten](http://nicolas.kruchten.com) at [Datacratic](http://datacratic.com). 


## What does it do?

Subtotal.js renders rows and columns of a pivot table with subtotals and lets the user to expand or collapse rows and columns.

![image](http://nagarajanchinnasamy.com/subtotal/images/subtotal-renderer-pivotui.png)

## Where can I see the demo?

You can see the live demo at [examples page](http://nagarajanchinnasamy.com/subtotal/examples/index.html).

## How do I load the code?

Subtotal.js implements the [Universal Module Definition (UMD)](https://github.com/umdjs/umd) pattern and so should be compatible with most approaches to script loading and dependency management: direct script loading with [RequireJS](http://requirejs.org/), [Browserify](http://browserify.org/) etc. For these options, you can grab it from [NPM](https://www.npmjs.com/package/subtotal) with `npm install subtotal` or via [Bower](http://bower.io/) with `bower install subtotal`. 

If you are loading the scripts directly (as in the [examples](http://nagarajanchinnasamy.com/subtotal)), you need to:

1. load the dependencies:
  1. jQuery in all cases
  2. jQueryUI for the interactive `pivotUI()` function (see below)
  3. D3.js, C3.js and/or Google Charts if you use [charting plugins](https://github.com/nicolaskruchten/pivottable/wiki/Optional-Extra-Renderers)
2. load the PivotTable.js files:
  1. `pivot.min.js`
3. load the Subtotal.js files:
  1. `subtotal.min.js`

Here is the source code of an [exmaple](https://github.com/nagarajanchinnasamy/pivottable-subtotal-renderer/blob/master/examples/subtotal_pivot.html/) 

## How do I use the code?

You can use Subtotal.js with either `pivot()` or `pivotUI()` method of PivotTable.js.

### To use `pivot()` method

1. Set the value of `dataClass` parameter to `$.pivotUtilities.SubtotalPivotData` 
2. Set the value of `renderer` parameter to `$.pivotUtilities.subtotal_renderers[<*rendererName*>]`
3. Optionally, set `rendererOptions`

```javascript
$(function(){
    $.getJSON("mps.json", function(mps) {
        $("#output").pivot(mps, {
            dataClass: $.pivotUtilities.SubtotalPivotData,
            rows: ["Gender", "Province"],
            cols: ["Party", "Age"],
            renderer: $.pivotUtilities.subtotal_renderers["Table With Subtotal"],
            rendererOptions: {
                collapseRowsAt: 1,
                collapseColsAt: 0
            }
        });
    });
});
```

### To use `pivotUI()` method

1. Set the value of `dataClass` parameter to `$.pivotUtilities.SubtotalPivotData` 
2. Set the value of `renderers` parameter to `$.pivotUtilities.subtotal_renderers`
3. Set the value of `rendererName` parameter to one of the subtotal renderers name
4. Optionally, set `rendererOptions`

```javascript
$(function(){
    $.getJSON("mps.json", function(mps) {
        $("#output").pivotUI(mps, {
            dataClass: $.pivotUtilities.SubtotalPivotData,
            rows: ["Gender", "Province"],
            cols: ["Party", "Age"],
            renderers: $.pivotUtilities.subtotal_renderers,
            rendererName: "Table With Subtotal",
            rendererOptions: {
                collapseColsAt: 0
            }
        });
    });
});
```

### Parameter: rendererName

`rendererName` can take one of the following values:

    "Table With Subtotal"
    "Table With Subtotal Bar Chart"
    "Table With Subtotal Heatmap"
    "Table With Subtotal Row Heatmap"
    "Table With Subtotal Col Heatmap"

### Parameter: rendererOptions

**`rowSubtotalDisplay`** option can be used to control the way row-subtotals are displayed. It can take one of the following values:

    "Hide On Expand"
    "Enable"
    "Disable"

**`colSubtotalDisplay`** option can be used to control the way column-subtotals are displayed. It can take one of the following values:

    "Hide On Expand"
    "Enable"
    "Disable"

**`collapseRowsAt`** option can be set to a `string` value as one of the elements of `rows` array or to a `numeric` value as index of one of the elements of `rows` array. If this option is set, rows are collapsed at the given row attribute whenever the pivot table is (re)rendered.  The default behavior is to render all rows expanded initially (ie., no collapse).

**`collapseColsAt`** option can be set to a `string` value as one of the elements of `cols` array or to a `numeric` value as index of one of the elements of `cols` array. If this option is set, columns are collapsed at the given column attribute whenever the pivot table is (re)rendered. The default behavior is to render all columns expanded initially (ie., no collapse).

For both `collapseRowsAt` and `collapseColsAt` options, using the numerical index value is recommended as it results in a better usability during rerendering of the table after drang and drop of attributes.

**`arrowCollapsed`** and **`arrowExpanded`** options can be used to customize the default arrows ("\u25B6" and "\u25E2" respectively) displayed with subtotal row/column labels and pivot axis labels.

**`table.eventHandlers`** option is set to a dictionary of events and their callback functions. On the occurence of an event given in this option, the corresponding callback function is invoked by passing the table cell element, value of the element, filtering criteria to fetch matching records from data and `SubtotalPivotData` instance. See [this example](http://nagarajanchinnasamy.com/subtotal/examples/260_event_handlers.html).

### Subtotal Aggregators

`$.pivotUtilities.subtotal_aggregators` is a dictionary of aggregators specific to this plugin. Following are the names of the aggregators in this dictionary:

    "Count As Fraction Of Parent Row"
    "Count As Fraction Of Parent Column"
    "Sum As Fraction Of Parent Row"
    "Sum As Fraction Of Parent Column"

[Please refer to this example](http://nagarajanchinnasamy.com/subtotal/examples/237_as_fraction_of_parent_row.html).

## How can I build the code and run the tests?

To install the development dependencies, just run `npm install`, which will create a `node_modules` directory with the files required to run the [Gulp](http://gulpjs.com/) build system.

After modifying any of the `.coffee` files at the top of the repo, you can compile/minify the files into the `dist` directory by running `node_modules/gulp/bin/gulp.js`

Once that's done, you can point your browser to `tests/index.html` to run the [Jasmine](http://jasmine.github.io/) test suite. You can view the [current test results here](http://nagarajanchinnasamy.com/subtotal/tests).

The easiest way to modify the code and work with the examples is to leave a `node_modules/gulp/bin/gulp.js watch serve` command running, which will automatically compile the CoffeeScript files when they are modified and will also run a local web server you can connect to to run the tests and examples.

## How can I contribute?

Pull requests are welcome! Here are some [Contribution Guidelines](https://github.com/nagarajanchinnasamy/pivottable-subtotal-renderer/blob/master/CONTRIBUTING.md).

## I have a question, how can I get in touch?

Please first check the [issues](https://github.com/nagarajanchinnasamy/pivottable-subtotal-renderer/issues) that are already raised and if you can't find what you're looking for there, then please [create a GitHub Issue](https://github.com/nagarajanchinnasamy/pivottable-subtotal-renderer/issues/new). When creating an issue, please try to provide a replicable test case so that others can more easily help you.

## Copyright & Licence (MIT License)

Subtotal.js is © 2016 Nagarajan Chinnasamy, Mindtree, other contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
