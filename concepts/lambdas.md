# Lambdas

Carbon lambdas are context aware functions called on a message from a node type. They can be chained ie a lambda can trigger another lambda however there are some limitations. A message sent by a lambda will not be able to trigger the same lambda (recursion). There will also be a maximum depth limit of 10 - meaning the number of nested or chained lambda calls will be limited to 10. Lambdas must run under 300ms. "Best practice" target is 100ms. We expose a `ctx` variable and several functions to provide additional information to the lambda functions. Lambdas are fairly open ended, if a lambda can't do it properly then you should attach to one of the existing "integration" points so that a custom service can handle whatever additional logic.

When to use a lambda:
Lambdas are intended to provide additional functionality over rules. They can handle some complicated workflows and run additional business rules resulting in more then just a state change. Variables and custom complicated functions and workflows can be implemented in a lambda.

When to not use a lambda:
When you can use a rule. If your lambda is limited OR hard to maintain because of the nature of being triggered on a per message basis. Extremely complicated / custom integrations with 3rd party systems, and a need for complicated states. Finally if the lambda is resource intensive to where it would cause scale issues for carbon. If any of the more complicated scenarios occur the solution would be using a websocket integration point throughout carbon in combination with the API to provide a solution. 
´
Lambdas can be tested here:
TODO add swagger link or something ?

## Table Of Contents

- [Basics](#basics)
- [Context](#context)
- [Functions](#functions)
    - [daysToSeconds(days)](#daystosecondsdays)
    - [hoursToSeconds(hours)](#hourstosecondshours)
    - [log(args ... )](#logargs--)
    - [getCurrentDataByUniqueId(uniqueId, channelNames)](#getcurrentdatabyuniqueiduniqueid-channelnames)
    - [getHistoryDataByUniqueId(uniqueId, channelName, start, end)](#gethistorydatabyuniqueiduniqueid-channelname-start-end)
    - [getNodeMetadata(uniqueId)](#getnodemetadatauniqueid)
- [Testing](#testing)
    - [TestResult{}](#testresult)

## Basics

* Lambdas functions can currently be written in ES5 javascript.

```javascript
var x = 5
log(x) // 5
let x = 5 // INVALID
```

* console.log() and similar functions have been intercepted with log(). BEST PRACTICE - use log() anyway.

```javascript
console.log(5) // same as log(5)
log(5) // best practice
```

* [underscorejs](http://underscorejs.org) has been imported.

```javascript
if (_.VERSION) {
    log("underscore version " + _.VERSION + " has been loaded.") 
    // underscore version 1.4.4 has been loaded.
}
```

## Context

We have provided some basic message context initially to the lambda. Any additional context will need to be fetched through a provided function.

```javascript
log(JSON.stringify(ctx, null, 2))
/*
{
    "channelName": "tempchannel",
    "nodeTypeId": 1,
    "parentNodeUniqueId": "00:00:00:00:00",
    "timestamp": 1500591111,
    "uniqueId": "00:00:00:00:00",
    "value": 9000.1
}
*/
```

## Functions

There are several functions that are exposed to the lambda engine. Generally speaking there are two types of functions exposed to the javascript engine - utility helper functions and carbon centric functions.

### daysToSeconds(days)

Converts days to seconds.

#### Params

* days `float` - the number of days to convert.

#### Returns

seconds - `float` - the number of seconds in the days parameter.

#### Example

```javascript
var seconds = daysToSeconds(5)
log(seconds) // 432000
```

### hoursToSeconds(hours)

Converts hours to seconds.

#### Params

* hours `float` - the number of hours to convert.

#### Returns

seconds - `float` - the number of seconds in the hours parameter.

#### Example

```javascript
var seconds = hoursToSeconds(5)
log(seconds) // 18000
```

### log(args ... )

Logs the .toString() result of each arg passed to it. **best practice** use log() instead of console.log().

#### Params

* args `any` - the item(s) to log.

#### Example

```javascript
log("Planet bob is best planet", true, 1 ) // Planet bob is best planet true 1

var someObject = {
    someProperty: 1
}
log(someObject) // [object Object]
log(JSON.stringify(someObject, null, 2)) // {"someProperty":1}
```

### sendAsNodeByUniqueId(uniqueId, channelName, value, nodeTypeName, parentUniqueId)

Will send a message to the activation service as if it were a node.

#### Limitation

a message sent by a lambda will not be able to trigger the same lambda (recursion). There will also be a maximum depth limit of 10 - meaning the number of nested or chained lambda calls will be limited to 10.

#### Params

* uniqueId `string` - the unique id of the node for the message to be sent.
* channelName `string` - the channel for the message to be sent on. note: All channel names are lower case.
* value `bool|string|float` - the value to be sent, the value must be a float, bool or string.
* nodeTypeName `string` - the node type name for the message.
* parentUniqueId `string` - the parent unique id of the node for the message to be sent.

#### Returns

isSuccessful - `bool` - returns if the message was received successfully.

#### Example

```javascript
var isSuccessful = sendAsNodeByUniqueId('00:00:00:00:00', 'tempchannel', 9000.1, 'temp-node', '00:00:00:00:00')
log(isSuccessful) // true
```

### getCurrentDataByUniqueId(uniqueId, channelNames)

Will get the current data values for the channels provided in the list.

#### Params

* uniqueId `string` - the unique id of the node you would like the values for.
* channelNames `string[]` - the channel names you would like in the return set.

#### Returns
currentData - `object` - returns an object with each property being a channel name.

#### Example

```javascript
var currentData = getCurrentDataByUniqueId('00:00:00:00:00', ['tempchannel'])
log(JSON.stringify(currentData, null, 2))
/*
{
   "tempchannel":{
      "timestamp":"2017-01-01T00:00:01Z",
      "value":9000.1
   }
}
*/
```

### getHistoryDataByUniqueId(uniqueId, channelName, start, end)

Will return the historical data for a given node, channel, and time range.

#### Params

* uniqueId `string` - the unique id of the node you would like the values for.
* channelName `string` - the channel name you would like in the return set.
* start - `int` - the start date in unix format (seconds since epoch).
* end - `int` - the end date in unix format (seconds since epoch).

#### Returns

historicalData - `datapoint[]` - returns an array of datapoints. They are sorted asc by timestamp.

#### Example

```javascript
var start = Date.now() / 1000
var end = Date.now() / 1000 - daysToSeconds(900)
var historicalData = getHistoryDataByUniqueId('00:00:00:00:00', 'tempchannel', start, end)
log(JSON.stringify(historicalData, null, 2))
/*
[
    {
        "timestamp": "2016-12-02T00:00:01Z",
        "value": 60
    }, {
        "timestamp": "2016-12-04T00:00:01Z",
        "value": 55
    }, {
        "timestamp": "2016-12-05T00:00:01Z",
        "value": 90
    }, {
        "timestamp": "2017-01-01T00:00:01Z",
        "value": 9000.1
    }
]
*/
```

### getNodeMetadata(uniqueId)

Will get the current Metadata value for the node requested.

#### Params

* uniqueId `string` - the unique id of the node you would like the metadata for.

#### Returns
metadata - `object` - returns an object with each property being a top level metadata key.

#### Example

```javascript
var metadata = getNodeMetadata('00:00:00:00:00')
log(JSON.stringify(metadata, null, 2))
/*
{
    "someKey": "some value"
}
*/
```

## Testing

TODO swagger api test link.

Hitting the test endpoint will not trigger any additional side effects. For example calling things like sendAsNodeByUniqueId() will not actually send the resulting message. The messages that would have otherwise been sent will however be in the resulting output. Testing can be done by hitting the API. The api will respond with a series of text output sections. Each section is detailed below.

### Sections

* Stats
* Output
* Errors
* Messages Sent
