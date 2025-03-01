# chartjs-plugin-dragdata.js

A plugin for Chart.js >= 2.4.0

Makes data points draggable. Supports touch events.

![Drag Data Animation](https://user-images.githubusercontent.com/20703207/61458811-ab2da180-a96b-11e9-90ae-7fc3dbec444e.gif)

Online demos:  
[Single Y-Axis](https://chrispahm.github.io/chartjs-plugin-dragdata/), [Dual Y-Axis](https://chrispahm.github.io/chartjs-plugin-dragdata/dualAxis.html),[Small Chart](https://chrispahm.github.io/chartjs-plugin-dragdata/smallChart.html),[Bubble Chart](https://chrispahm.github.io/chartjs-plugin-dragdata/bubble.html),[Radar Chart](https://chrispahm.github.io/chartjs-plugin-dragdata/radar.html),[Bar Chart](https://chrispahm.github.io/chartjs-plugin-dragdata/bar.html),[Horizontal Bar Chart](https://chrispahm.github.io/chartjs-plugin-dragdata/horizontalBar.html),[Stacked Bar Chart](https://chrispahm.github.io/chartjs-plugin-dragdata/stackedBar.html), [Stacked Horizontal Bar Chart](https://chrispahm.github.io/chartjs-plugin-dragdata/stackedHorizontalBar.html),[React Fiddle](https://jsfiddle.net/3v9kexbh/)

## Installation

### npm

```
npm install chartjs-plugin-dragdata
```

### CDN
In browsers, you may use the following script tag:
```
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-dragdata@1.0.1/dist/chartjs-plugin-dragdata.min.js"></script>
```

Or, download a release archive file from the dist folder.

## Configuration

To make (line, bubble, bar and radar chart) data points draggable, simply add ```dragData: true``` to the config section of the chart instance. If you (additionally to the y-axis) would like to drag data along the x-axis, you may also add ```dragX: true```.

To round the values dragged to, simply add ```dragDataRound: 0``` to the config section of the chart instance.
 * `0` will round to `..., -2, -1, 0, 1, 2, ...`
 * `1` will round to `..., -0.2, -0.1, 0.0, 0.1, 0.2, ...`
 * `-1` will round to `..., -20, -10, 0, 10, 20, ...`

Individual event listeners can be specified as follows:

```javascript
const myChartOptions = {
  type: 'line', // or radar, bar, horizontalBar, bubble
  data: {...}, 
  options: {
    ... // the rest of your chart options, e.g. axis configuration
    dragData: true,
    dragX: false,
    dragDataRound: 0,
    onDragStart: function (e, element) {
      // where e = event
    },
    onDrag: function (e, datasetIndex, index, value) {
      // where e = event
    },
    onDragEnd: function (e, datasetIndex, index, value) {
      // where e = event
    }
  }
}
```

Minimum and maximum allowed data values can be specified through the `min` and `max` ticks settings in the scales options. By setting these values accordingly, unexpected (fast) changes to the scales, that may occur when dragging data points towards the outer boundaries of the y-axis, can be prohibited.

```javascript
const myChartOptions = {
  type: 'line', // or radar, bar, horizontalBar, bubble
  data: {...}, 
  options: {
    dragData: true,
    scales: {
      yAxes: [{
        ticks: {
          max: 25,
          min: 0
        }
      }]
    },
    ... // the remainder of your chart options, e.g. dragData: true etc.
}
```

To avoid dragging specific datasets, you can set dragData to false within the dataset options.

```javascript
const myChartOptions = {
  type: 'line', // or radar, bar, horizontalBar, bubble
  data: {
    datasets: [
      {
        label: "Data Label",
        fill: false,
        data: dataPoints,
        yAxisID: 'B',
        dragData: false
      }, {
    ...
  },
  options: {
    dragData: true,
    ... // the remainder of your chart options
  }
}
```

To avoid dragging specific scales, you can set dragData to false within the axis scale options.

```javascript
const myChartOptions = {
  type: 'line', // or radar, bar, horizontalBar, bubble
  data: {...}, 
  options: {
    dragData: true,
    scales: {
      yAxes: [{
        dragData: false
      }]
    },
    ... // the remainder of your chart options, e.g. dragData: true etc.
}
```

To avoid dragging specific data points inside a draggable dataset, you can return `false` to function `onDragStart`.

To disable the automatic update of the data, you can return `false` to function `onDrag`. Nothing will happen to the points without you changing the `data` attribute somewhere else. This is useful for frameworks like emberjs who us the data down action up paradigm.

## Touch devices
In order to support touch events, the [`pointHitRadius`](https://www.chartjs.org/docs/latest/charts/line.html#point-styling) option should be set to a value greater than `25`. You can find working example configurations in the `docs/*.html` files. Also note, that mobile devices (and thus touch events) can be simulated with the [device mode in the Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/device-mode/).

## Gotchas
When working with a module bundler (e.g. Webpack) and a framework (e.g. Vue.js/React/Angular), you still need to import the plugin library after installing. 
Here's a small example for a Vue.js component

```js
<template>
  <div>
    <canvas id="chart"></canvas>
  </div>
</template>
<script>
import Chart from 'chart.js'
// load the options file externally for better readability of the component.
// In the chartOptions object, make sure to add "dragData: true" etc.
import chartOptions from '~/assets/js/labour.js'
import 'chartjs-plugin-dragdata'

export default {
  data() {
    return {
      chartOptions
    }
  },
  mounted() {
    this.createChart('chart', this.chartOptions)
  },
  methods: {
    createChart(chartId, chartData) {
      const ctx = document.getElementById(chartId)
      const myChart = new Chart(ctx, {
        type: chartData.type,
        data: chartData.data,
        options: chartData.options,
      })
    }
  }
}
</script>
<style>
</style>
```
## Contributing

Please feel free to submit an issue or a pull request!
If you make changes to the src/index.js file, don't forget to `npm run build` and
manually test your changes against all demos in the docs folder.

## License

chartjs-plugin-dragdata.js is available under the [MIT license](http://opensource.org/licenses/MIT).
