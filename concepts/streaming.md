# General Overview

The carbon api has a web-socket resource `api/stream` that exposes a real time feed of data points and events. There are three endpoints:

* `/api/stream/:id/data` - opens up one web-socket that will publish all data points for the node `:id`. To unsubscribe simply close the socket.

* `/api/stream/:id/event` - opens up one web-socket that will publish all events for the node `:id`. To unsubscribe simply close the socket.

* `/api/stream/all` - is a single web-socket that will allow the consumer to subscribe, and unsubscribe to multiple node's events and/or data points. It also supports channel filtering.

### `/api/stream/:id/data`

You can subscribe to a specific node's data feed using the following command:
Example Subscription:
```bash
TODO: POST /api/stream/:id/data UPGRADE
```

After you have subscribed the socket will receive a json payload whenever a data point is received in carbon.

Example Payload:
```json
{
  "parentNodeId": 1,
  "parentNodeUniqueId": "unq_1_1",
  "nodeTypeId": 1,
  "nodeId":2,
  "uniqueId": "unq_2_2",
  "channelName": "some_channel",
  "timestamp": "",
  "value": "some value"
}
```

### `/api/stream/:id/event`

You can subscribe to a specific node's event feed using the following command:
Example Subscription:
```bash
TODO: POST /api/stream/:id/event UPGRADE
```

After you have subscribed the socket will receive a json payload whenever a change occurs due to a rule in carbon.

Example Payload:
```json
{
  "nodeTypeId": 1,
  "nodeId": 1,
  "channelName": "some_channel",
  "timestamp": "",
  "value": "some_value",
  "ruleId": 1,
  "change": ""
}
```

### `/api/stream/all`

The `stream/all` is similar to the other endpoints except it allows the consumer to receive data, and events from multiple nodes through the same web-socket.

After the initial handshake with the web-socket, the consumer will need to instruct the server as to what it wants to receive. This is done via a `request` payload.

Example `request` payload:

```json
{
  "action":"subscribe:data",
  "arguments": {
    "nodeId": 1,
    "channelName": "some_channel"
  }
}
```

#### Available actions

* `subscribe:data` - will subscribe to a specific node's datapoints.
* `unsubscribe:data` - will unsubscribe a node from the socket feed.
* `subscribe:event` -  will subscribe to a node's events.
* `unsubscribe:event` - will unsubscribe from a node's events.

#### Available Arguments

Arguments allow the consumer to specify filters for a particular subscription. They are passed in via the `arguments` object in the request.

For data subscriptions:

* `nodeId` - Required - Specify to filter the data stream based on node id.
* `channelName` - Optional - Specify to filter the data stream based on a channel.

For event subscriptions:

* `nodeId` - Required - Specify to filter the event stream based on node id.

Once the consumer is subscribed to a node the server will return an `response` object.

#### Response Payload

The response json the server returns will contain the action type the message is for and a few status indicators.

Example `response` payload:

```json
{
  "type":"acknowledge",
  "statusCode": 200,
  "statusMessage": "subscribed to event stream for node id 5",
  "payload": {}
}
```

#### Available payload types

* `acknowledge` - this is an acknowledge response to notify the consumer of the success status of their request.
* `data` - this is a payload with a datapoint from a node.
* `event` - this is a payload type with an event that resulted from a state change.

Example data payload:

```json
{
  "parentNodeId": 1,
  "parentNodeUniqueId": "unq_1_1",
  "nodeTypeId": 1,
  "nodeId":2,
  "uniqueId": "unq_2_2",
  "channelName": "some_channel",
  "timestamp": "",
  "value": "some value"
}
```

Example event payload:

```json
{
  "nodeTypeId": 1,
  "nodeId": 1,
  "channelName": "some_channel",
  "timestamp": "",
  "value": "some_value",
  "ruleId": 1,
  "change": ""
}
```