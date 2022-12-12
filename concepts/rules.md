# Rules

Carbon rules allow basic on/off for a node state. All rule conditions must return a single value: true or false. 

Simple examples of Rules include:

- *if temperature is greater than 100 degrees*
- *if battery life is less than 10*
- *if the average value of a channel is greater than some number*

**When to use a Rule**  
Rules are intended to be small and simple and result in a node state change. Simple math, boolean operations and window function calls are allowed, but all executions must result in true or false. When possible, always use a Rule for changing node state, as they're much cheaper to scale than lambdas.

**When to not use a Rule**  
When you need custom variables, complicated formulas/functions or complicated business logic, you're better off using a lambda. 

# Table of Contents

- [Basics](#basics)
- [Context](#context)
- [Functions](#functions)
  - [min(n1, n2)](#minn1-n2)
  - [max(n1, n2)](#maxn1-n2)
  - [contains(str1, str2)](#containsstr1-str2)
  - [notContains(str1, str2)](#notcontainsstr1-str2)
  - [channelUniqueCount(channelName, lookBackMinutes)](#channeluniquecountchannelname-lookbackminutes)
  - [channelCount(channelName, lookBackMinutes)](#channelcountchannelname-lookbackminutes)
  - [channelMax(channelName, lookBackMinutes)](#channelmaxchannelname-lookbackminutes)
  - [channelMin(channelName, lookBackMinutes)](#channelminchannelname-lookbackminutes)
  - [channelSum(channelName, lookBackMinutes)](#channelsumchannelname-lookbackminutes)
  - [channelAverage(channelName, lookBackMinutes)](#channelaveragechannelname-lookbackminutes)
  - [channelCountByUniqueID(nodeuid, channelName, lookBackMinutes)](#channelcountbyuniqueidnodeuid-channelname-lookbackminutes)
- [Testing](#testing)

## Basics

Rules conditions are executed with a markup language called "Wilma," which has a simple syntax described below. 

* Wilma can evaluate basic math, comparison operators, and follows order of operations. The following example(s) will result with true.

```javascript
1 + 1 == 2
1 != 2 * 1
2 >= 4 / 2
1 + 2 * 5 == 11
(1 + 2) * 5 == 15
```

* Wilma can do basic boolean algebra. The following example(s) will result with true.

```javascript
true OR false
true AND true
```

* Wilma can use functions. The following example(s) will result with true.

```javascript
min(1,2) == 1
```

* Wilma has context variables provided. The following example will result with true, assuming the somechannel's value is greater then 0. (You cannot set custom variables in wilma. If you need this functionality, you're better off using a lambda.)

```javascript
somevar.value > 0
```

* Wilma accepts double or single quotes. The following example(s) will result with true.

```javascript
contains("somethinng", "s")
contains('somethinng', 's')
```

* Wilma functions can call other functions or use a context variable. The following example(s) will result with true, assuming the somechannel's value is 1.

```javascript
min(2,somechannel.value) == 1
max(1,max(1,2)) == 2
```

* Wilma depends on spacing to differentiate negative numbers and subtraction operations. Negative numbers must not have a space between the `-` sign and the number itself. Subtraction operators must have a space between the `-` operator and the operands.

```javascript
1 - 1 == 0  // true
1 - -1 == 2 // true
1-1 == 0    // invalid
1 - - 1     // invalid
```

* Wilma numbers are evaluated as floats. The following example(s) will result with true.

```javascript
1.00 == 1
min(1,2) == 1.0
min(1,2) == 1
```

* Wilma keywords, function names and context variables are case insensitive. Best practice: be consistent. The following example(s) will result with true.

```javascript
min(1,2) == MIN(1,2)
numberchannel.value == numberChannel.value
1 == 1 AND 1 == 1 and 1 == 1
```

* Wilma can support any combination of the above operations. The following example will result with true, assuming the value of 'numberchannel' is 1.

```javascript
min(1,2) == MIN(1,2) AND numberchannel.value * 5 == 1 * 5
```

## Context

Rules are executed with context variables. The context provided is mostly current channel datapoint values. These are accessed via the channel name with `.value` or `.timestamp` attached. The timestamp is an epoch number.

Examples:

```javascript
somechannel.value
somechannel.timestamp
```

### Utility Context

Included with the current channel context are helper variables. Currently there is only one variables as seen below: 

```javascript
now       // this is the current time in unix epoch seconds (UTC)
occupied  // true when node's folder has an occupancy schedule and folder is occupied at datapoint's timestamp
```

## Cron & Debounce Timers

There are two types of timers that can affect rule execution, as denoted by the cron and debounce fields in the Rules edit UI. The cron timer determines whether the current rule should run as a cron job (cron time in minutes). This can be set to 0 for a real-time rule, or between 30 and 10080 minutes for a cron rule. Cron rules are also evaluated on that channel when regular data is processed. The debounce timer can be set between 0 and 60, and will wait that number of seconds after a positive rule result to react to the rule, and cancel the result if the rule evaluates to false during the wait period. Setting the debounce field to 0 results in no debounce.

#### Limits

```javascript
cronTime => 0 OR 30 - 10080 (minutes)
debounceTime => 0 - 60 (seconds)
```

## Functions

There are several functions that are exposed to the rule execution engine. Generally speaking, these functions fall into two categories: utility helper functions and Carbon-centric window functions. Function string parameters can be used with single or double quotes. You are permitted to use context variables or functions as parameters. **There are no comments in wilma** &mdash; comments used in the examples below with `//` are just for documentation and will need to be removed for execution.

### Functions Available

- [min(n1, n2)](#minn1-n2)
- [max(n1, n2)](#maxn1-n2)
- [contains(str1, str2)](#containsstr1-str2)
- [notContains(str1, str2)](#notcontainsstr1-str2)
- [channelUniqueCount(channelName, lookBackMinutes)](#channeluniquecountchannelname-lookbackminutes)
- [channelCount(channelName, lookBackMinutes)](#channelcountchannelname-lookbackminutes)
- [channelMax(channelName, lookBackMinutes)](#channelmaxchannelname-lookbackminutes)
- [channelMin(channelName, lookBackMinutes)](#channelminchannelname-lookbackminutes)
- [channelSum(channelName, lookBackMinutes)](#channelsumchannelname-lookbackminutes)
- [channelAverage(channelName, lookBackMinutes)](#channelaveragechannelname-lookbackminutes)
- [channelCountByUniqueID(nodeuid, channelName, lookBackMinutes)](#channelcountbyuniqueidnodeuid-channelname-lookbackminutes)

### min(n1, n2)

Returns the minimum of the two numbers.

#### Params

- n1 `float` - the left number for comparison.
- n2 `float` - the right number for comparison.

#### Returns

result - `float` - the minimum value of the two floats provided. 

#### Example

```javascript
min(1,2) == 1.0 // true
min(1,2) == 2 // false
```

### max(n1, n2)

Returns the max of the two numbers.

#### Params

- n1 `float` - the left number for comparison.
- n2 `float` - the right number for comparison.

#### Returns

result - `float` - the max value of the two floats provided. 

#### Example

```javascript
max(1,2) == 2.0 // true
max(1,2) == 1 // false
```

### contains(str1, str2)

Returns true if str2 is found present in str1.

#### Params

- str1 `string` - the string to look into.
- str2 `string` - the string to look for.

#### Returns

isFound - `bool` - whether or not str1 was found in str2.

#### Example

```javascript
contains("abcd","a") // true
contains("abcd","e") // false
contains("abcd","abc") // true
```

### notContains(str1, str2)

Returns true if str2 is not found present in str1.

#### Params

- str1 `string` - the string to look into.
- str2 `string` - the string to look for..

#### Returns

isFound - `bool` - whether or not str1 was found in str2.

#### Example

```javascript
notContains("abcd","a") // false
notContains("abcd","e") // true
notContains("abcd","abc") // false
```

### channelUniqueCount(channelName, lookBackMinutes)

Returns the number of unique values for a given channel name and time period.

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelUniqueCount - `int` - the number of unique values for the given channel during the time period.

#### Example

```javascript
channelUniqueCount('numberchannel',525600) > 1 // true
```

### channelCount(channelName, lookBackMinutes)

Returns the number values for a given channel name and time period.

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelCount - `int` - the number of values for the given channel during the time period.

#### Example

```javascript
channelCount('numberchannel',525600) > 1  // true
```

### channelMax(channelName, lookBackMinutes)

Returns the max value for a given channel name and time period. Note: only works with channels that are of type "number".

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelMax - `float` - the max value of the channel during the time period.

#### Example

```javascript
channelMax('numberchannel',525600) > 1  // true
```

### channelMin(channelName, lookBackMinutes)

Returns the min value for a given channel name and time period. Note: only works with channels that are of type "number".

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelMin - `float` - the min value of the channel during the time period.

#### Example

```javascript
channelMin('numberchannel',525600) < 10000  // true
```

### channelSum(channelName, lookBackMinutes)

Returns the sum of all the values for a given channel name and time period. Note: only works with channels that are of type "number".

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelSum - `float` - the sum of all the values of the channel during the time period.

#### Example

```javascript
channelSum('numberchannel',525600) > 1  // true
```

### channelAverage(channelName, lookBackMinutes)

Returns the average value for a given channel name and time period. Note: only works with channels that are of type "number".

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelAverage - `float` - the average value of the channel during the time period.

#### Example

```javascript
channelAverage('numberchannel',525600) > 1  // true
```

#### Example

```javascript
channelAverage('numberchannel',525600) > 1  // true
```

### channelCountByUniqueID(nodeUID, channelName, lookBackMinutes)

Returns the number values for a given node unique id, channel name, and time period.

#### Params

- nodeUniqueID `string` - the unique id of the node to analyze.
- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelCount - `int` - the number of values for the given channel during the time period.

#### Example

```javascript
channelCountByUniqueID( gateway.value , 'numberchannel',525600) > 1 // true
```

## Testing

*Coming soon.*

Wilma rules can be tested via the API or the UI. The user will be given the opportunity to provide context for the rule. In order to test the rule the user will need to reference an existing node with a valid node type. This will be used to provide context for the rule and historical data. The user will need permissions to all referenced resources.

The result will return the following information:

* runtime - `int` - the number of ms the rule took to execute.
* result - `bool` - the result of the rule execution.
* errors - `[]string` - any errors produced by the execution.
* timedout - `bool` - true if the operation timed out.

API:

```
TODO:
```

UI:

```
TODO:
```
