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

```
min() todo more complex
```

### max(n1, n2)

Returns the max of the two numbers.

#### Params

- n1 `float` - the left number for comparison.
- n2 `float` - the right number for comparison.

#### Returns

result - `float` - the max value of the two floats provided. 

#### Example

```
max() todo more complex
```

