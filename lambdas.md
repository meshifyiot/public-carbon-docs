
# <a name = "Functions"> Functions </a>

There are several functions that are exposed to the lambda js engine. Generally speaking there are two types of functions exposed to the javascript engine - utility helper functions and carbon centric functions. The [underscorejs]() library is also imported.

## daysToSeconds(days)

Converts days to seconds.

### Params

- days `float` - the nubmber of days to convert.

### Returns

seconds - `float` - the number of seconds in the days paramter.

### Example

```javascript
var seconds = daysToSeconds(5)
log(seconds) // TODO
```

## hoursToSeconds(hours)

Converts hours to seconds.

### Params

- hours `float` - the nubmber of hours to convert.

### Returns 

seconds - `float` - the number of seconds in the hours parameter.

### Example

```javascript
var seconds = hoursToSeconds(5)
log(seconds) // TODO
```

## log(args ... )

Logs the .toString() result of each arg passed to it.

### Params

- args `any` - the item(s) to log.

```javascript
log("Planet bob is best planet", true, 1 ) // Planet bob is best planet true 1

var someObject = {
    someProperty: 1
}
log(someObject) // [object Object]
log(JSON.stringify(someObject)) // {"someProperty":1}
```

## sendAsNodeByUniqueId(uniqueId, channelName, value, nodeTypeName, parentUniqueId)

Will send a message to the activation service as if it were a node.

### Limiation

a message sent by a lambda will not be able to trigger the same lambda (recursion). There will also be a maximum depth limit of 10 - meaning the number of nested or chained lambda calls will be limited to 10.

### Params

- uniqueId `string` - the unique id of the node for the message to be sent.
- channelName `string` - the channel for the message to be sent on. note: All channel names are lower case.
- value `bool|string|float` - the value to be sent, the value must be a float, bool or string.
- nodeTypeName `string` - the node type name for the message.
- parentUniqueId `string` - the parent unique id of the node for the message to be sent.

### Returns:

isSuccessful - `bool` - returns if the message was received succesfully.

### Example:

```javascript
var isSuccessful = sendAsNodeByUniqueId('00:00:00:00:00', 'tempchannel', 9000.1, 'temp-node', '00:00:00:00:00')
log(isSuccessful) // true
```

## getCurrentDataByUniqueId(uniqueId, channelNames) 

Will get the current data values for the channels provided in the list.

### Params

- uniqueId `string` - the unique id of the node you would like the values for.
- channelNames `string[]` - the channel names you would like in the return set.

### Returns
currentData - `object` - returns an object with each property being a channel name.

### Example

```javascript
var currentData = getCurrentDataByUniqueId('00:00:00:00:00', ['tempchannel'])
log(currentData)
/*
{
    tempchannel: 9000.1
}
*/
```

## getHistoryDataByUniqueId(uniqueId, channelName, start, end)

Will return the historical data for a given node, channel, and time range.

### Params

- uniqueId `string` - the unique id of the node you would like the values for.
- channelName `string` - the channel name you would like in the return set.
- start - `int` - the start date in unix format (seconds since epoch).
- end - `int` - the end date in unix format (seconds since epoch).

### Returns

historicalData - `datapoint[]` - returns an array of datapoints.

### Example

```javascript
var currentData = getHistoryDataByUniqueId('00:00:00:00:00', 'tempchannel', )
log(currentData)
/*
{
    tempchannel: 9000.1
}
*/
```
