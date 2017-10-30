# Nodes and Node Types

In the Carbon platform a Node is a virtual representation of your hardware. A Node also has a Node Type which can be changed at anytime.

A Node contains Channels, which are time streams with a specified name. So you could have a "temperature" channel defined on a Node Type and each Node of that Node Type would have its own "temperature" time stream. Then, when you queries for the Node's "temperature" over the last 3 weeks, you would get a list of values and timestamps of the Node's "temperature" readings for the last 3 weeks.

## Node Type

If you are a programmer, a Node Type can be thought of a sort of "view"
into the Carbon Platform data model. It provide a definition of the Channels availabe on each Node of that Node Type as well as some common metadata like Name (used in MQTT), Vanity Name, and roles that are allowed to view Node's of that Node Type. For Advanced Users, it also provides access to auto-activation configuration (see below).

This "view" also contains other common configuration parameters. All Rules, Node Type Templates, and Lambdas are tied directly to a Node Type, so switching a Node's Node Type will also change the Rules, Node TYpe Templates, and Lambdas applied to that Node Type.

### Channels

Channels are defined using a JSON syntax. With this you can define the referencable channel name, vanity name, default value, and data type (number, string, or boolean). You can also add metadata to each channel definition. An example channel definition is below:

```
{
    "channels": {
        "haspower": {
            "default": false,
            "type": "boolean",
            "vanityName":"testname5",
            "metadata": {
                "channelId": 17821
            }
        },
        "status": {
            "default": null,
            "type": "string",
            "vanityName":"testname6"
        },
        "temp": {
            "default": 1,
            "type": "number",
            "vanityName":"testname6"
        }
    }
}
```

### Alias Channels

Alias channels are a more complex topic than Channels, but they are configured on the Node Type in much the same way. An Alias Channel is a Channel that does not have data (a time-stream) of its own, but references its from another Node's data. This is similar to how you can have a directory link in a file system (an alias that points to a directory somewhere else).

What this allows you to do is setup a situation where you have a Node of Node Type "house", where "house" has 3 "alias:number" Channels called "bedroomTemp", "bathroomTemp", and "garageTemp". Now, this Node does not have a physical piece of hardware representing it, rather there are 3 other Nodes of Node Type "tempsensor" with 1 "number" Channel called "temp".

What we would do is use `/api/aliaschannels` to create a link from the "house" Node and "bedroomTemp" Channel to one of the "tempsensor" Node's "temp" Channel. Then, when you queries `api/data/history` for the "bedroomTemp" Channel on the "house" Node, you would actually be getting the history of the "tempsensor" Node's "temp" channel. You can also trigger Rules and Lambdas in real time on the "house" Node against the original data from the "tempsensor" Node.

What you've now done is created an aggregation of 3 different "tempsensor"s that gives your users a much better contextual representation of what they are trying to represent. This also allows you to more easily do cross-Node Rules and Lambdas, so that you could trigger an alert based on the temperature difference in two rooms.

#### Alias Channel Validation

Alias channels have a few rules that must be followed: 

* Alias channels must be defined in the schema with an `alias:` type.
* A channel with the `alias:` type cannot be the source of another alias channel.
* Alias channels are not allowed to have overlapping start/end times. 
* There can only be one transition within a 24 hour period. 

### Auto-Activation Config

TBD

## Nodes

TBD

### Aliasing

TBD

