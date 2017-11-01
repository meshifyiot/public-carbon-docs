## Node Type Templates: Line Chart

### Introduction

For data visualizations, Meshify offers the Highcharts and Highstocks libraries, both of which have [extensive documentation available](https://www.highcharts.com/docs). We have created several custom tags for the most common chart types that you can use inside your Node Type templates. Here's how to create a basic line chart.

### Syntax

[*Highcharts demo.*](https://www.highcharts.com/demo/line-basic)

The simplest line chart can be added as follows:

```
<sample-template>

	<mf-chart start_time={ moment().subtract(7, 'days').format() } channels="channel1,channel2" label="Test Chart" />

</sample-template>

```

This will create a standard line chart with a week's worth of data for channels channel1 and channel2. The 'channels' string is a comma-separated list of channel names. There are two in this example, but you can have as many as you want.

Often, however, you'll like to customize the chart more directly, whether in the colors, labels, display, etc. Meshify offers an alternative way to configure it:

```
<sample-template>
	<mf-chart settings={ chart_settings } />

	<script>

		var tag = this;
		tag.chart_settings = {
			channels: [
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
			],
			label: "Chart",
			simple: true,
			start_time: moment().subtract(30, 'days').format(),
		}

	</script>

</sample-template>

```

This is just a sampling of the properties you can supply to the chart configuration object. Here is the full list of options.

---

**channels**

Either a comma-separated list of channel names, or an array of objects conforming to the example above.

The channel objects can include the following properties:

- name: The name of the channel
- label: The vanity name of the channel to show in the chart legend
- axis: Left or right, for which y-axis the data should be drawn on
- axis_label: An optional label to render on the chosen axis
- unit: The unit for each data point, rendered in tooltips
- color: The HTML/CSS color used to render the data, labels, etc.

All of these properties except 'name' are optional, with strong defaults.

*No default.*

*Only the comma-separated list of channels can be passed in separately without a settings object.*

---

**height**

An integer. The height (in pixels) of the final chart.

*Defaults to 400. If the simple property is set to true, shrinks to 300.*

---

**label**

A string for the label that appears at the very top of the chart.

*No default.*

*Label can also be passed in separately from the settings object.*

---

**sampling**

A boolean. By default, Highstocks samples data values into larger blocks to increase chart readability and performance. Data points that are close to one another (measured in pixels) are grouped and averaged, but will separate into individual points when you zoom into the chart. 

*Defaults to true.*

---

**simple**

A boolean. Simple mode strips out a lot of the accessory UI elements and offers a simple chart with a legend, the data points, and little else. It's ideal for list templates, where vertical space is limited.

*Defaults to false.*

---

**start_time**

An ISO8601 format timestamp. For the sake of your sanity, please use a Moment.js format() function call.

*Defaults to one week ago.*

*Start_time can be passed in separately from the settings object.*
