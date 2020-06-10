# Reactions
Reactions are the result of a state change from a rule. Currently a reaction will be either an sms, an email, or [HTTP POST request webhook](./concepts/webhooktemplates). The body, subject, url, from email, and from name fields are all templateable and will default to tenant settings if they are set or meshify's platform defaults. Reactions have a deduplication mechanism, where the the exact same message will only be sent out once every 90 seconds. 

## Templates

Templates are primarily used for reaction emails and sms notifications. The templating engine in use is called [pongo2](https://github.com/flosch/pongo2), which is a port of Django 1.7 template engine. Please note django documentation was used and is referenced in here. When browsing django template documentation please note that this is only for v1.7. Message context has also been provided.

## Quick Start

Example template:

```django
{% for key, value in currentAlarms %}
Alarm: {{ key }} : {{ value.ruleName }} : {{ value.updatedAt }}
{% endfor %}

{% comment %} This checks to see if alias1channel is in the currentData map via 'dot' syntax {% endcomment %}
{% if currentData.alias1channel %}
It is in the current data dictionary.
{% endif %}

{% comment %} This checks to see if the alias1channel is in the currentData map via the getItem filter, after it checks to see if the key "alias1channel" is in the map.{% endcomment %}
{% if "alias1channel" in currentData %}
{% with test=currentData|getItem:"alias1Channel" %}
{{ test.value }}
{% endwith %}
{% endif %}

{% comment %} This is how you access an array item directly.{% endcomment %}
{{ arrayOfItems.0.Name }} 
  
{% comment %} Another way to access items in an array {% endcomment %}
{% with arrayOfItems|first as firstItem %}
    {{ firstItem.Name }}
{% endwith %}
``` 


Output using example context:
```
Alarm: 3 : Example Rule Name : 2017-11-01T11:22:27.515676-05:00
```


It is in the current data dictionary.


```
1.400000
``` 


## Context

This is an example of accessing a context variable:

```django
{{ ctx.channelName }}
```

This is an example of the context object:

```json
{
    "ctx": {
        "change": "enter",
        "channelName": "temp",
        "nodeId": 15,
        "nodeTypeId": 7,
        "ruleId": 15,
        "tenantId": 1,
        "timestamp": 1509552965
    },
    "currentAlarms": {
        "3": {
            "ruleName": "Example Rule Name",
            "state": 1,
            "updatedAt": "2017-11-01T11:22:27.515676-05:00"
        }
    },
    "currentData": {
        "alias1channel": {
            "timestamp": "2017-11-01T11:22:27.515676-05:00",
            "value": 1.4,
            "vanityName": "Vanity Name Channel"
        }
    },
    "folder": {
        "createdAt": "2017-10-30T16:28:41.161646Z",
        "folderPath": "A/B/CV/D/",
        "id": 18,
        "information": {
            "address": {
                "address": "5418 New Rd.",
                "code": "343",
                "country": "",
                "locality": "706 W Ben White Blvd",
                "region": "Austin"
            }
        },
        "isRoot": false,
        "location": null,
        "metadata": {},
        "name": "D",
        "parentFolderId": 6,
        "tags": [],
        "theme": {},
        "updatedAt": "2017-10-30T16:28:41.161646Z"
    },
    "node": {
        "area": "janitor's closet",
        "building": "Frost Tower",
        "createdAt": "2017-10-30T16:28:41.161646Z",
        "floor": "10th",
        "folderId": 18,
        "id": 15,
        "isActive": true,
        "location": null,
        "metadata": {},
        "nodeTypeId": 7,
        "parentNodeId": 4,
        "tags": [],
        "uniqueId": "c4:93:00:03:6b:24:00:45",
        "updatedAt": "2017-10-30T16:28:41.161646Z",
        "vanityName": "Test Node 15"
    },
    "nodeType": {
        "activationConfig": {
            "autoActivate": true,
            "autoCreate": true,
            "changeNodeType": false,
            "changeParent": false,
            "setFolder": "parent",
            "setParent": "sender",
            "syncParentFolder": false
        },
        "allowedRoles": null,
        "channels": {
            "alias1channel": {
                "default": 1,
                "metadata": null,
                "type": "alias:number",
                "vanityName": ""
            },
        },
        "createdAt": "2017-10-30T16:28:41.161646Z",
        "driverId": {
            "Int64": 0,
            "Valid": false
        },
        "id": 7,
        "name": "m7",
        "updatedAt": "2017-10-30T16:28:41.161646Z",
        "vanityName": ""
    },
    "parentNode": {
        "area": "northeast corner",
        "building": "Frost Tower",
        "createdAt": "2017-10-30T16:28:41.161646Z",
        "floor": "12th",
        "folderId": 18,
        "id": 15,
        "isActive": true,
        "location": null,
        "metadata": {},
        "nodeTypeId": 7,
        "parentNodeId": 4,
        "tags": [],
        "uniqueId": "c4:93:00:03:6b:24:00:45",
        "updatedAt": "2017-10-30T16:28:41.161646Z",
        "vanityName": "Test Node 15"
    },
    "reaction": {
        "id": 6,
        "name": "Test reaction 3",
        "ruleId": 15,
        "type": "sms"
    },
    "recipient": {
        "address": {
            "address": "",
            "code": "",
            "country": "",
            "locality": "",
            "region": ""
        },
        "email": "email@example.com",
        "firstName": "",
        "lastName": "",
        "notificationSchedule": "",
        "phone": "",
        "preferences": {
            "temp_units": "F",
            "favorite_color": "blue"
        }
    },
    "theme": {}
}
```

## Expressions

Pongo supports basic expressions. Operators include:

```
==, !=, <, <=, >, >=, &&, and, !, not, ||, or, in, not in
```

Examples:

```django
integers and complex expressions
{{ 10-100 }}
{{ -(10-100) }}
{{ -1 * (-(-(10-100)) ^ 2) ^ 3 + 3 * (5 - 17) + 1 + 2 }}

floats
{{ 5.5 }}
{{ 5.172841 }}
{{ 5.5 - 1.5 == 4 }}
{{ 5.5 - 1.5 == 4.0 }}

mul/div
{{ 2 * 5 }}
{{ 2 * 5.0 }}
{{ 2 * 0 }}
{{ 2.5 * 5.3 }}
{{ 1/2 }}
{{ 1/2.0 }}
{{ 1/0.000001 }}

logic expressions
{{ !true }}
{{ !(true || false) }}
{{ true || false }}
{{ true or false }}
{{ false or false }}
{{ false || false }}
{{ true && (true && (true && (true && (1 == 1 || false)))) }}

float comparison
{{ 5.5 <= 5.5 }}
{{ 5.5 < 5.5 }}
{{ 5.5 > 5.5 }}
{{ 5.5 >= 5.5 }}

remainders
{{ (simple.number+7)%7 }}
{{ (simple.number+7)%7 == 0 }}
{{ (simple.number+7)%6 }}

in/not in
{{ 5 in simple.intmap }}
{{ 2 in simple.intmap }}
{{ 7 in simple.intmap }}
{{ !(5 in simple.intmap) }}
{{ not(7 in simple.intmap) }}
{{ 1 in simple.multiple_item_list }}
{{ 4 in simple.multiple_item_list }}
{{ !(4 in simple.multiple_item_list) }}
{{ "Hello" in simple.misc_list }}
{{ "Hello2" in simple.misc_list }}
{{ 99 in simple.misc_list }}
{{ False in simple.misc_list }}
```

## Filters

You can modify variables for display by using filters. Filters look like this: `{{ name|lower }}`. This displays the value of the `{{ name }}` variable after being filtered through the lower filter, which converts text to lowercase. Use a pipe (|) to apply a filter.

Filters can be “chained.” The output of one filter is applied to the next. `{{ text|escape|linebreaks }}` is a common idiom for escaping text contents, then converting line breaks to `<p>` tags.

Some filters take arguments. A filter argument looks like this: `{{ bio|truncatewords:30 }}`. This will display the first 30 words of the bio variable.

Filter arguments that contain spaces must be quoted; for example, to join a list with commas and spaced you’d use `{{ list|join:", " }}`. 

There are a few [caveats](https://github.com/flosch/pongo2#caveats) regarding django format strings vs python format strings. The library is using go's format strings style for the [time format](https://golang.org/pkg/time/#Time.Format) and [string format](https://golang.org/pkg/fmt/) strings.

### Example

Filters are functions, often used for formatting things.

```django
{{ 3500000|filesizeformat }}
```

Output:

```html
3.3MiB
```

### Available Filters

* [escape](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#escape)
* [safe](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#safe)
* [escapejs](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#escapejs)
* [add](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#add)
* [addslashes](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#addslashes)
* [capfirst](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#capfirst)
* [center](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#center)
* [cut](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#cut)
* [date](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#date)
* [default](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#default)
* [default_if_none](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#default-if-none)
* [divisibleby](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#divisibleby)
* [first](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#first)
* [floatformat](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#floatformat)
* [get_digit](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#get-digit)
* [iriencode](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#iriencode)
* [join](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#join)
* [last](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#last)
* [length](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#length)
* [length_is](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#length-is)
* [linebreaks](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#linebreaks)
* [linebreaksbr](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#linebreaksbr)
* [linenumbers](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#linenumbers)
* [ljust](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#ljust)
* [lower](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#lower)
* [make_list](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#make-list)
* [phone2numeric](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#phone2numeric)
* [pluralize](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#pluralize)
* [random](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#random)
* [removetags](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#removetags)
* [rjust](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#rjust)
* [slice](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#slice)
* [split](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#split)
* [stringformat](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#stringformat)
* [striptags](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#striptags)
* [time](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#time)
* [title](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#title)
* [truncatechars](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#truncatechars)
* [truncatechars_html](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#truncatechars-html)
* [truncatewords](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#truncatewords)
* [truncatewords_html](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#truncatewords-html)
* [upper](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#upper)
* [urlencode](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#urlencode)
* [urlize](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#urlize)
* [urlizetrunc](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#urlizetrunc)
* [wordcount](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#wordcount)
* [wordwrap](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#wordwrap)
* [yesno](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#yesno)

### Pongo Specific Filters

* float
* integer

### 3rd Party Filters
* [filesizeformat](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#filesizeformat)
* [slugify](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#slugify)
* truncatesentences
* truncatesentences_html
* markdown
* [intcomma](https://docs.djangoproject.com/en/1.7/ref/contrib/humanize/#intcomma)
* [ordinal](https://docs.djangoproject.com/en/1.7/ref/contrib/humanize/#ordinal)
* [natrualday](https://docs.djangoproject.com/en/1.7/ref/contrib/humanize/#natrualday)
* [timesince](https://docs.djangoproject.com/en/1.7/ref/contrib/humanize/#timesince)
* [timeuntil](https://docs.djangoproject.com/en/1.7/ref/contrib/humanize/#timeuntil)
* [naturaltime](https://docs.djangoproject.com/en/1.7/ref/contrib/humanize/#naturaltime)

### Custom Filters

* [getItem](#getItem(inmap[string]interface,keystring)interface{})

#### getItem(in map[string]interface, key string) interface{}

This function will get return the value of a key from a map.

Example:

```django
{% set value=someMap|getItem:"someKey" %}
```

## Tags

Tags look like this:{% raw  %} `{% tag %}` {% endraw %}. Tags are more complex than variables: Some create text in the output, some control flow by performing loops or logic, and some load external information into the template to be used by later variables. Some tags require beginning and ending tags (i.e. `{% raw %} {% tag %} {% endraw %} ... tag contents ... {% raw  %} {% tag %} {% endraw %}`).

### Example

For loop over json object keys using the example context above.

```django
{% for key, value in currentAlarms %}
{{ key }} : {{ value.ruleName }} : {{ value.updatedAt }}
{% endfor %}
```

Output:

```html
3 : Example Rule Name : 2017-11-01T11:22:27.515676-05:00
```

### Available Tags

* [autoescape](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#autoescape)
* [block](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#block)
* [comment](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#comment)
* [cycle](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#cycle)
* [filter](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#filter)
* [firstof](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#firstof)
* [for](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#for)
* [if](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#if)
* [ifchanged](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#ifchanged)
* [ifequal](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#ifequal)
* [ifnotequal](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#ifnotequal)
* [now](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#now)
* [spaceless](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#spaceless)
* set
* [templatetag](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#templatetag)
* [widthratio](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#widthratio)
* [with](https://docs.djangoproject.com/en/1.7/ref/templates/builtins/#with)
