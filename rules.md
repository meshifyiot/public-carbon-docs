# Rules

Carbon rules allow basic on off for a node state. They are currently executed with will's markup language "Wilma". Rules are intended to be lighter weight then lambdas. Rules must run under 100ms. "Best practice" target is 1ms. We expose several context variables including current channel data values. As well as several functions.

When to use a Rule:
TODO EXAMPLES - compare against a lambda. IE what can I do with a rule vs lambda

When to not use a lambda:
TODO EXAMPLES

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
- [Testing](#testing)

## Basics

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
{min(1,2)} == 1
```

* Wilma has context variables. The following example(s) will result with true. Assuming the somechannel's value is greater then 0.

```javascript
{.somechannel.value} > 0
```

* Wilma accepts double or single qoutes. The following example(s) will result with true.

```javascript
{contains("somethinng", "s")}
{contains('somethinng', 's')}
```

* Wilma depends on spacing to differentiate negative numbers and subtraction operations. Negative numbers must not have a space beween the `-` sign and the number itself. Subtraction operators must have a space between the `-` operator and the opperands.

```javascript
1 - 1 == 0  // true
1 - -1 == 2 // true
1-1 == 0    // invalid
1 - - 1     // invalid
```

* Wilma numbers are evaluated as floats. The following example(s) will result with true.

```javascript
1.00 == 1
{min(1,2)} == 1.0
{min(1,2)} == 1
```

* Wilma keywords, function names and context variables are case insensitive. Best practice: be consistent. The following example(s) will result with true.

```javascript
{min(1,2)} == {MIN(1,2)}
{.numberchannel.value} == {.numberChannel.value}
1 == 1 AND 1 == 1 and 1 == 1
```

* Wilma can support any combination of these things. The following example(s) will result with true. Assuming the numberchannels value is 1.

```javascript
{min(1,2)} == {MIN(1,2)} AND {.numberchannel.value} * 5 == 1 * 5
```

## Context

Rule execution will have access to context variables. These variables are accessed similar to functions via brackets. The context provided is mostly channel datapoint values. These are accessed via the channel name with `.value` or `.timestamp` attatched. A shortcut to access the current channel values is avaliable via `.current`. The timestamp is an epoc utc timestamp.

Examples:

```javascript
{.somechannel.value}
{.somechannel.timestamp}
{.current.value}
{.current.timestamp}
```

## Functions

There are several functions that are exposed to the rule execution engine. Generally speaking there are two types of functions exposed to the engine - utility helper functions and carbon centric window functions. Function parameters can be used with no qoutes, single or double qoutes. You are permitted to use context variables in function parameters however you are not allowed to use a function calls as a parameter. If there is a use case for this please let us know the exact use case with an example. There are no comments in wilma, comments used in the examples are just for documentation and will need to be removed for execution.

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

## min(n1, n2)

Returns the minumim of the two numbers.

#### Params

- n1 `float` - the left number for comparison.
- n2 `float` - the right number for comparison.

#### Returns

result - `float` - the minimum value of the two floats provided. 

#### Example

```javascript
{min(1,2)} == 1.0 // true
{min(1,2)} == 2 // false
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
{max(1,2)} == 2.0 // true
{max(1,2)} == 1 // false
```

### contains(str1, str2)

Returns true if str2 is found present in str1.

#### Params

- str1 `string` - the string to look into.
- str2 `string` - the string to look for.

#### Returns

isFound - `bool` - weather or not str1 was found in str2.

#### Example

```javascript
{contains("abcd","a")} // true
{contains("abcd","e")} // false
{contains("abcd","abc")} // true
```

### notContains(str1, str2)

Returns true if str2 is not found present in str1.

#### Params

- str1 `string` - the string to look into.
- str2 `string` - the string to look for..

#### Returns

isFound - `bool` - weather or not str1 was found in str2.

#### Example

```javascript
{notContains("abcd","a")} // false
{notContains("abcd","e")} // true
{notContains("abcd","abc")} // false
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
{channelUniqueCount('numberchannel',525600)} > 1 // true
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
{channelCount('numberchannel',525600)} > 1  // true
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
{channelMax('numberchannel',525600)} > 1  // true
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
{channelMin('numberchannel',525600)} < 10000  // true
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
{channelSum('numberchannel',525600)} > 1  // true
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
{channelAverage('numberchannel',525600)} > 1  // true
```

## Testing

Wilma rules can be tested via the API or the UI. The user will be given the oppurtunity to provide context for the rule. In order to test the node the user will need to reference an existing node with a valid node type. This will be used to provide context for the rule and historical data. The user will need permissions to all referenced resources.

The result will return the following information:

* runTime - `int` - the number of ms the rule took to execute.
* result - `bool` - the result of the rule execution.
* errors - `[]string` - any errors produced by the execution.
* timedout - `bool` - true if the operation timedout.

API:

```
TODO:
```

UI:

```
TODO:
```