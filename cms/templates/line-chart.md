## Node Type Templates: Line Chart

### Introduction

For data visualizations, Meshify offers the Highcharts and Highstocks libraries, both of which have [extensive documentation available](https://www.highcharts.com/docs). We have created several custom tags for the most common chart types that you can use inside your Node Type templates. Here's how to create a basic line chart.

### Syntax

[*Highcharts demo.*](https://www.highcharts.com/demo/line-basic)

The simplest line chart can be added as follows:

`<mf-chart start_time={ moment().subtract(7, 'days').format() } channels="channel1,channel2" simple="true">`

This will create a standard line chart with a week's worth of data for channels channel1 and channel2. The 'simple' property is optional, but if set to true, tells the chart to leave out a lot of the more advanced features. The 'channels' string is a comma-separated list of channel names. There are two in this example, but you can have as many as you want.

Often, however, you'll like to customize the chart more directly, whether in the colors, labels, display, etc. Meshify offers an alternative way to supply the channels:

```
<mf-chart start_time={ moment().subtract(7, 'days').format() } channels={ chart_channels }>

<script>

	var tag = this;
	tag.chart_channels = [
		{
			name: 'hum',
			label: 'Humidity',
			axis: 'left',
			axis_label: 'Humidity',
			unit: '%',
			color: '#E28F48'
		},
		{
			name: 'temp',
			label: 'Temperature',
			axis: 'right',
			axis_label: 'Temperature',
			unit: 'deg',
			color: '#93C9F4'
		}
	];

</script>

```

By passing in an array of channel objects, you can fine-tune the way the chart visualizes them. The objects can include the following properties:

- name: The name of the channel
- label: The vanity name of the channel to show in the chart legend
- axis: Left or right, for which y-axis the data should be drawn on
- axis_label: An optional label to render on the chosen axis
- unit: The unit for each data point, rendered in tooltips
- color: The HTML/CSS color used to render the data, labels, etc.

All of these properties except 'name' are optional, with strong defaults.

The `<mf-chart>` tag has several customizable options that you can pass into the tag. Here is the full list of properties.

---

**channels**

Either a comma-separated list of channel names, or an array of objects conforming to the example above.

*No default.*

---

**start_time**

An ISO8601 format timestamp. For the sake of everyone's sanity, please use a Moment.js format() function call.

*Defaults to one week ago.*

---

**label**

A string for the label that appears at the very top of the chart.

*No default.*

---

**simple**

A boolean string. Simple mode strips out a lot of the accessory UI elements and offers a simple chart with a legend, the data points, and little else. It's ideal for list templates, where vertical space is limited.

*Defaults to false.*