# Rules

Carbon rules allow basic on off for a node state. They are based off willma (will's markup language).Rules are intdeded to be lighter weight then lambdas. Rules must run under 100ms. "Best practice" target is 1ms. We expose several context variables including current channel data values. As well as several basic functions and window functions. All functions must resolve to a single value, and all rules must resolve to true or false.

When to use a Rule:
TODO EXAMPLES - compare against a lambda. IE what can I do with a rule vs lambda 

When to not use a lambda:
TODO EXAMPLES

Lambdas can be tested here:
TODO add swagger link or something ?

## Context

TODO

## Functions

There are several functions that are exposed to the rule execution engine. Generally speaking there are two types of functions exposed to the engine - utility helper functions and carbon centric window functions.

### min(n1, n2)

Returns the minumim of the two numbers.

#### Params

- n1 `float` - the left number for comparison.
- n2 `float` - the right number for comparison.

#### Returns

result - `float` - the minimum value of the two floats provided. 

#### Example

```javascript
{min(1,2)} == 1.0 // true
{min(1,2)} == 1 // true
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
{max(1,2)} == 2 // true
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
{contains(abcd,a)} // true
{contains(abcd,e)} // false
{contains(abcd,abc)} // true
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

```

### channelUniqueCount(channelName, lookBackMinutes)

Returns the number of unique values for a given channel name and time period.

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelUniqueCount - `int` - the number of unique values for the given channel during the time period.

#### Example

```
channelUniqueCount() todo more complex
```

### channelCount(channelName, lookBackMinutes)

Returns the number values for a given channel name and time period.

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelCount - `int` - the number of values for the given channel during the time period.

#### Example

```
channelCount() todo more complex
```

### channelMax(channelName, lookBackMinutes)

Returns the max value for a given channel name and time period. Note: only works with channels that are of type "number".

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelMax - `float` - the max value of the channel during the time period.

#### Example

```
channelMax() todo more complex
```

### channelMin(channelName, lookBackMinutes)

Returns the min value for a given channel name and time period. Note: only works with channels that are of type "number".

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelMin - `float` - the min value of the channel during the time period.

#### Example

```
channelMin() todo more complex
```

### channelSum(channelName, lookBackMinutes)

Returns the sum of all the values for a given channel name and time period. Note: only works with channels that are of type "number".

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelSum - `float` - the sum of all the values of the channel during the time period.

#### Example

```
channelSum() todo more complex
```

### channelAverage(channelName, lookBackMinutes)

Returns the average value for a given channel name and time period. Note: only works with channels that are of type "number".

#### Params

- channelName `string` - the name of the channel to analyze.
- lookBackMinutes `int` - the number of minutes from now to look back.

#### Returns

channelAverage - `float` - the average value of the channel during the time period.

#### Example

```
channelAverage() todo more complex
```