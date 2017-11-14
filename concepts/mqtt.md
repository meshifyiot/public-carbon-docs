# MQTT

MQTT Messages will be ingested if the `uniqueId` and `parentNodeId` are present on a given `tenantId`. Data will be accepted on channels, even if they don't exist, and it can be added to the schema at a later time.

## MQTT v1 (Current Platform)

### Topic Structure - Backwards-Compatible Legacy Format

`meshify/db/:tenantId/:parentNodeId/:techName/:channelName`

### Topic Structure - Carbon Format

`meshify/db/:tenantId/:parentNodeId/:nodeTypeName/:uniqueId/:channelName`

Notes:

- You can replace `:parentNodeId` with `_` if you want the node to be its own parent.

#### Special Channels

- `mfy_n_loc` (Meshify Node Location) - will update the node's `.location` field with new gps coordinates 
- `mfy_f_loc` (Meshify Folder Location) - will update the folder's `.location` field with new gps coordinates

GPS Coordinate Format:

```
[{"value":{"lat":0.0, "lng":0.0}}]
```

### Payload Structure

```
[{"value": true, "timestamp": 1234578}]
[{"value": "hello", "timestamp": 1234578}]
[{"value": 12.5, "timestamp": 1234578}]
[{"value": true}]
```

Notes:

- Omitting the timestamp or setting it to 0 will replace it with the current time in UTC when the message arrives to the server
- JSON can be sent as the `"value"`, but you will not be able to write Rules against it (this may change in the future)

## Version 2

Expected Q4 2017.
