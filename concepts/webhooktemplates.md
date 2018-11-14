# Webhook Reaction Templates

## Templating

The template engine is the same as the rest of the platform: pongo2. More information on this is available in [reactions](/concepts/reactions.md#templates). In addition to the Webhook body, the URL and Header are also templateable.

## URL Template

You may not point the URL to the Carbon APIs.  If you seek to do so, please use [Lambdas](/concepts/lambdas.md) instead.

##### URL Example using [Context](/concepts/reactions.md#context) variables
```
http://www.example.com/api/node/{{ctx.nodeId}}/
```

## Header Templates Format

You may use the header template to add headers to your Webhook Reaction.  This field is expected to be a JSON encoded key-value consisting of string values.  By default, the request `Content-Type` is set to `application/json`

##### Authentication Example
```
{"Authentication":"Basic ZXhhbXBsZXVzZXJuYW1lOnBhc3N3b3Jk"}
```
##### Example using numbers and [Context](/concepts/reactions.md#context) variables
```
{"Age":"24","Content-Type":"application/json","User-Agent":"Node Transport {{ctx.nodeID}}"}
```

## Body Template

You may have the body be any text format you would like it to be as long as you change the `Content-Type`.  By default for webhooks, `"Content-Type":"application/json"` is set in the headers.  There is no other validation of the Body template besides it must be less than 5000 characters long.  You may refer to [Reactions Quick Start](/concepts/reactions.md#quick-start) for and example of what functions are available to use
