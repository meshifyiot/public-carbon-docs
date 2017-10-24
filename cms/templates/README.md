# Node Type Templates

### Introduction

Node Type templates are an opportunity to visualize the data streaming from your nodes for your end users. As a reminder, all nodes in Meshify have a Node Type, which provide a channel schema and corresponding Node Type templates. 

Inside these templates, you can output the current value for the node's channels, create forms to send data back down to the node, and offer charts and many other data visualizations. This section will introduce each feature and highlight Meshify's built-in template components.

### Syntax

Node Type Templates are written in the standard languages of the web: HTML, CSS, and Javascript. Meshify uses [Riot.js](http://riotjs.com/) as its chief templating language, which offers many of the benefits of React with a simpler syntax. Each Node Type Template is technically a Riot tag, which does have a few requirements:

- The template must be wrapped in an outer closing tag that matches the name of the template, i.e. `<template-name></template-name>`.
- Those outer closing tags **must be unique.** Multiple Riot tags with the same outer tag name will overwrite one another in the order in which they are loaded.

### Structure

Each Node Type in Meshify has 1 list template, i.e. `nodetypename-list`, and N number of detail templates, i.e. `nodetypename-templatename`. 

The standard Meshify Dashboard application has two chief views: the main list view, which is a list of all nodes, and a singular node view, wherein you've clicked on a node in the list view to see more information about it.

The list template is visible in the main list view, and is best suited for summary data or highlighting the most important channels on the node. The singular node view is where you can see any and all detail templates for the device.

### Context

Node Type templates have access to:

- [Lodash](https://lodash.com/docs/4.17.4)
- [Tachyons](http://tachyons.io/docs/)
- [Riot.js](http://riotjs.com)
- [Moment.js](http://momentjs.com)

You also have access to key information about the node, passed in as a Riot.js 'opts' object:

```javascript

	alarms: [],
	channels: {
		temp: {
			value: 100,
			timestamp: "2017-01-01T00:00:01Z",
			vanityName: "temperature"
		},
		...
	},
	icon: {
		name: "Default",
		level: 0,
		shape: "circle",
		color: "green"
	},
	id: 0,
	location: {
		lat: 0,
		lng: 0
	},
	metadata: {},
	parentNodeId: 0,
	tags: [],
	uniqueId: "00:00:00:00",
	updatedAt: "2017-09-22T16:12:26.967Z"
	vanity: "Node Name"

```
*All properties are accessible at opts.node. Example: opts.node.channels.temp.value.*

### Sample Template

Here's an example of a sample detail template, which outputs all of the channel values for a node:

```javascript

	<sample-template>

		<div each={ channel_name, point in opts.node.channels }>
			<h2>{ point.vanityName }</h2>
			<p>{ point.value }</p>
			<time>{ moment(point.timestamp).format('MMMM Do YYYY, h:mm:ss a') }</time>
		</div>

	</sample-template>

	<script>
		var tag = this;
	</script>

```

### Available Functions

*Coming soon.*