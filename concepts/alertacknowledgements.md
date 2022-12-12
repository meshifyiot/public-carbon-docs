# Alert Acknowledgements

## Obtaining Token in Reaction Templates

In order to obtain the response token, checking the reactions for `responseTokens` is suggested in order to prevent loss of the reaction in event it does not get populated.  Please see [Reactions](./reactions.md) for more details

The token exists in the variable `{{ responseTokens.Code }}`

Example:

```{% if responseTokens %}Click to acknowledge: https://meshi.fyi/{{ responseTokens.Code }}{% endif %}```

#### Link Shortening

We currently use `https://meshi.fyi/{token}` as our link shortening domain in order to redirect users to `https://acknowledge.meshifyprotect.com/`

## Responding With Token

#### Step 1. Retrieve Token Info

In order to retrieve alarm info and check if the token still is valid, use [GET /api/v2/alarms/response/{code}](https://api-docs.meshify.co/#/Alarms/ApiService_AlarmStatus)

Example Response
```
{
  "location": "string",
  "name": "string",
  "type": "string",
  "footerHelp": "string",
  "reaction": "string",
  "metadata": {},
  "alarmTs": "2022-12-12T13:45:16.559Z",
  "supportInfo": {
    "email": "string",
    "phone": "string"
  },
  "icon": {
    "name": "string",
    "description": "string",
    "level": 0,
    "primaryShape": "string",
    "primaryColor": "string"
  }
}
```

#### Step 2. Giving a response.

In order to respond to a token, the API endpoint is [POST /api/v2/alarms/response/acknowledge](https://api-docs.meshify.co/#/Alarms/ApiService_Acknowledge)

This requires either the account phone or email of the user to validate who is acknowledging.  This prevents someone accidentally acknowledging for a location they do not belong to since this method is unauthenticated otherwise.

Example Body
```
{
  "code": "string",
  "phone": "555555555",
  "type": "ack"
}
```

Valid Types
```
ack - Acknowledge, will pause future reactions if configured
test - Ignore / Testing, will pause future reactions if configured
decline - Decline Action, but allow alarms to continue even if configured to pause on Acknowledgement
```