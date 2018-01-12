## Node Type Templates: History Table

### Introduction

A convenient way to view channel histories for multiple channels at once is through a table. Meshify offers a built-in history table component for just this purpose — provide it a list of channels, and it'll loop through all of them, find the common timestamps, and render a table showing each channel's value at those timestamps. 

**The history table is intended for channels that update simultaneously from the device. Timestamps not shared amongst all the provided channels are ignored.**

### Syntax

The simplest history table can be added as follows:

```
<sample-template>

	<mf-history-table start_time={ moment().subtract(7, 'days').format() } channels="channel1,channel2" />

</sample-template>

```

This will create a standard history table with a week's worth of data for channels channel1 and channel2. The 'channels' string is a comma-separated list of channel names. There are two in this example, but you can have as many as you want.

Meshify also offers an alternative way to configure the table:

```
<sample-template>
	<mf-history-table settings={ table_settings } />

	<script>

		var tag = this;
		tag.table_settings = {
			channels: [
				{
					name: 'hum',
					label: 'Humidity',
					unit: '%',
				},
				{
					name: 'temp',
					label: 'Temperature',
					unit: 'deg',
				}
			],
			start_time: moment().subtract(1, 'days').format(),
		}

	</script>

</sample-template>

```

Here is the full list of properties that can be supplied to the settings object.

---

**channels**

Either a comma-separated list of channel names, or an array of objects conforming to the example above.

The channel objects can include the following properties:

- name: The name of the channel
- label: The header for this channel's column in the table
- unit: The unit for each data point, rendered after the value

All of these properties except 'name' are optional, with strong defaults.

*No default.*

*Only the comma-separated list of channels can be passed in separately without a settings object.*

---

**height**

An integer. The height (in pixels) of the final table. Scrolling is enabled by default if the number of rows exceeds what can be rendered with the configured height.

*Defaults to 500.*

---

**timezone**

A string for the timezone in which to render all timestamps in the table. [A whole table of valid timezone strings can be found on Wikipedia](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

*Defaults to moment.tz.guess(), which guesses (fairly accurately) the timezone of the logged-in user.*

---

**start_time**

An ISO8601 format timestamp. For the sake of your sanity, please use a Moment.js format() function call.

*Defaults to one week ago.*

*Start_time can be passed in separately from the settings object.*
