<script>
  import { onMount, afterUpdate, onDestroy } from 'svelte';
  import { Chart } from 'frappe-charts/dist/frappe-charts.min.cjs.js';

  /**
   *  PROPS
   */ 
  export let data = {
    labels: [],
    datasets: [
      { values: [] }
    ],
    yMarkers: {},
    yRegions: [],
  };
  export let type = 'line';
  export let height = 300;
  export let axisOptions = {};
  export let barOptions = {};
  export let lineOptions = {};
  export let tooltipOptions = {};
  export let colors = [];
  export let valuesOverPoints = 0;
  export let isNavigable = false;
  export let maxSlices = 3;

  /**
   *  COMPONENT
   */
  //  The Chart returned from frappe
  let chart = null;
  //  DOM node for frappe to latch onto
  let chartRef;

  //  Helper HOF for calling a fn only if chart exists
  function ifChartThen(fn) {
    return function ifChart(...args) {
      if (chart) {
        return fn(...args);
      }
    }
  }

  /**
   * Methods for updating / exporting the chart
   */
  //  Allow the consumer to add a data point
  export const addDataPoint = ifChartThen((label, valueFromEachDataset, index) => chart.addDataPoint(label, valueFromEachDataset, index));

  //  Allow the consumer to remove a data point
  export const removeDataPoint = ifChartThen(index => chart.removeDataPoint(index));

  //  Allow the consumer to export the chart
  export const exportChart = ifChartThen(() => chart.export());

  /**
   *  Handle initializing the chart when this Svelte component mounts 
   */
  onMount(() => {
    chart = new Chart(chartRef, {
      data,
      type,
      height,
      colors,
      axisOptions,
      barOptions,
      lineOptions,
      tooltipOptions,
      valuesOverPoints,
      isNavigable,
      maxSlices,
    });
  });

  //  Update the chart when incoming data changes
  afterUpdate(() => chart.update(data));
  
  //  Mark Chart references for garbage collection when component is unmounted
  onDestroy(() => {
    chart = null;
  });
</script>


<div bind:this={chartRef}></div>
