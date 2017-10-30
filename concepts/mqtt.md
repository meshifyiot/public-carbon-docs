# MQTT

## Legacy

### Topic Structure

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
