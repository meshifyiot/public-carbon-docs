# Reactions

## Templates

Templates are primarily used for reaction emails and sms notifications. The templating engine in use is called [pongo2](https://github.com/flosch/pongo2), which is a port of Django 1.7 template engine. Please note django documentation was used and is referenced in here. When browsing django template documentation please note that this is only for v1.7. Message context has also been provided.

## Quick Start

TODO

## Context

This is an example of accessing a variable.

```
{{ rec }}
```

```javascript
// TODO 
	rec := map[string]interface{}{
		"firstName":            subscription.FirstName,
		"lastName":             subscription.LastName,
		"phone":                subscription.Phone,
		"email":                subscription.Email,
		"notificationSchedule": subscription.Schedule,
	}

	m := map[string]interface{}{
		"tenantId":    msg.TID,
		"nodeTypeId":  msg.NodeTypeID,
		"nodeId":      msg.NodeID,
		"channelName": msg.ChannelName,
		"timestamp":   msg.Timestamp,
		"ruleId":      msg.RuleID,
		"change":      msg.Change.String(),
		"reactionId":  reaction.ID,
	}

	nodeInfo := map[string]interface{}{
		"uniqueId":   nodeContext.UniqueID,
		"vanityName": nodeContext.VanityName,
	}

	return map[string]interface{}{
		"ctx":       m,
		"recipient": rec,
		"node":      nodeInfo,
	}
```

## Expressions

Pongo supports basic expressions. TODO

## Filters

You can modify variables for display by using filters. Filters look like this: `{{ name|lower }}`. This displays the value of the `{{ name }}` variable after being filtered through the lower filter, which converts text to lowercase. Use a pipe (|) to apply a filter.

Filters can be “chained.” The output of one filter is applied to the next. `{{ text|escape|linebreaks }}` is a common idiom for escaping text contents, then converting line breaks to `<p>` tags.

Some filters take arguments. A filter argument looks like this: `{{ bio|truncatewords:30 }}`. This will display the first 30 words of the bio variable.

Filter arguments that contain spaces must be quoted; for example, to join a list with commas and spaced you’d use `{{ list|join:", " }}`. 

There are a few [caveats](https://github.com/flosch/pongo2#caveats) regarding django format strings vs python format strings. The library is using go's format strings style for the [time format](https://golang.org/pkg/time/#Time.Format) and [string format](https://golang.org/pkg/fmt/) strings.

### Common Example

```
TODO
```

Renders

```html
TODO
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

## Tags

Tags look like this:{% raw  %} `{% tag %}` {% endraw %}. Tags are more complex than variables: Some create text in the output, some control flow by performing loops or logic, and some load external information into the template to be used by later variables. Some tags require beginning and ending tags (i.e. `{% raw %} {% tag %} {% endraw %} ... tag contents ... {% raw  %} {% tag %} {% endraw %}`).

### Common Examples

```
```

Renders

```html
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
