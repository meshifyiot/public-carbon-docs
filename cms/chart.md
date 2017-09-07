## Node Type Template Charts

### Line Chart

Available as `<mf-chart label="Temp" start_time="2017-01-01T00:00:01Z" channels="channel1,channel2" simple="true">`

- channels: array of channel names to show in the chart
- label: title of the chart
- start_time (optional): start time of history query (defaults to 5 minutes from right now)
    - uses ISO8601 format, so you may want to do `start_time={ moment().subtract(30, 'minutes').format() }`
- simple (optional): show a simple graph (generally used for list view)
