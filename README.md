<div align="center" margin="0 auto 20px">
  <h1>svelte-frappe-charts</h1>
  <p style="font-style: italic;">📈 Svelte bindings for <a href="https://frappe.io/charts">frappe-charts.</a></p>
  <div>
    <a href='https://travis-ci.org/himynameisdave/svelte-frappe-charts'>
        <img src="https://api.travis-ci.org/himynameisdave/svelte-frappe-charts.svg?branch=master" alt="Travis Badge" />
    </a>
    <a href="https://app.fossa.io/projects/git%2Bgithub.com%2Fhimynameisdave%2Fsvelte-frappe-charts?ref=badge_shield" alt="FOSSA Status">
      <img src="https://app.fossa.io/api/projects/git%2Bgithub.com%2Fhimynameisdave%2Fsvelte-frappe-charts.svg?type=shield"/>
    </a>
    <a href="https://www.npmjs.com/package/svelte-frappe-charts">
        <img src="https://img.shields.io/npm/dm/svelte-frappe-charts.svg" alt="Downloads">
    </a>
    <a title="MadeWithSvelte.com Shield" href="https://madewithsvelte.com/p/svelte-frappe-charts/shield-link">
      <img src="https://madewithsvelte.com/storage/repo-shields/2274-shield.svg" />
    </a>
  </div>
</div>

---


Makes it easy to use [frappe-charts](https://frappe.io/charts) in your [Svelte](https://svelte.dev/) project.

### Installation

```
yarn add svelte svelte-frappe-charts

npm i -S svelte svelte-frappe-charts
```

> **Note**: Assumes you are using `>= svelte@3.0.0`.

### Usage

Use the chart in your Svelte project with ease:

```svelte
<script>
  import Chart from 'svelte-frappe-charts';

  let data = {
    labels: ['Sun', 'Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat'],
    datasets: [
      {
        values: [10, 12, 3, 9, 8, 15, 9]
      }
    ]
  };
</script>

<Chart data={data} type="line" />
```

The component API directly matches the [the configuration of `frappe-charts`](https://frappe.io/charts/docs/reference/configuration).

### Updating data

There are two ways to update data in a chart: either in adding and removing individual points, or updating the existing data with an entirely new set of data points.

#### Updating individual data points

##### addDataPoint

Add a data point to the chart, increasing the length of the dataset.

```svelte
<script>
  import Chart from 'svelte-frappe-charts';

  let data = {
    labels: ['Sun', 'Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat'],
    datasets: [
      {
        values: [10, 12, 3, 9, 8, 15, 9]
      }
    ]
  };

  let chartRef;

  function addDataPoint() {
    chartRef.addDataPoint('Wed', [30], 1);
  }
</script>

<Chart data={data} type="line" bind:this={chartRef} />

<button on:click={addDataPoint}>Add data point</button>
```

[More info on `addDataPoint`.](https://frappe.io/charts/docs/reference/api#adddatapoint)

##### removeDataPoint

Remove a data point from the chart, reducing the length of the dataset.

```svelte
<script>
  import Chart from 'svelte-frappe-charts';

  let data = {
    labels: ['Sun', 'Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat'],
    datasets: [
      {
        values: [10, 12, 3, 9, 8, 15, 9]
      }
    ]
  };

  let chartRef;

  function removeDataPoint() {
    chartRef.removeDataPoint(3); // Index of the item to remove
  }
</script>

<Chart data={data} type="line" bind:this={chartRef} />

<button on:click={removeDataPoint}>Remove data point</button>
```

[More info on `removeDataPoint`.](https://frappe.io/charts/docs/reference/api#removedatapoint)

#### Updating full data

Update the entire data, including annotations, by passing the entire new data object to update.

```svelte
<script>
  import Chart from 'svelte-frappe-charts';

  let data = {
    labels: ['Sun', 'Mon', 'Tues', 'Wed', 'Thurs', 'Fri', 'Sat'],
    datasets: [
      {
        values: [10, 12, 3, 9, 8, 15, 9]
      }
    ]
  };

  let chartRef;

  function updateData() {
    data = {
      labels: ['Friday', 'Saturday', 'Sunday'],
      datasets: [
        {
          values: [300, 380, 275]
        }
      ]
    };
  }
</script>

<Chart data={data} type="line" bind:this={chartRef} />

<button on:click={updateData}>Update Chart</button>
```

### Exporting charts

You can easily export a chart ([see Exporting](https://frappe.io/charts/docs/exporting/images)) as an SVG by storing a reference to the `<Chart />` component, and then calling `exportChart` on it:

```svelte
<script>
  // ...

  let chartRef;
  const onExport = () => chartRef.exportChart();
</script>

<Chart data={data} type="line" bind:this={chartRef} />
<button on:click={onExport}>
  Export
</button>
```

### Contributing

[Issues](https://github.com/himynameisdave/svelte-frappe-charts/issues/new) and pull requests are greatly welcome!

---

_✌️Created by [Dave](http://himynameisdave.com). Licenced under MIT._
