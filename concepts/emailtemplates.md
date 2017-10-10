# Email Templates

## Basics

Email Templates are templates used for emails sent by the Meshify Platform during certain events. They are in no way related to Alarms, Reactions, or Notifications.

Currently there are two events which make use of Email Templates:

- User Invite (triggered by `POST /api/authentication/membership`)
- Password Reset (triggered by `/api/passwords/resetpassword?email=someemail@somewhere.com`)

These are accessible for you to edit at:

- `PUT /api/emailtemplates/users-invite` ()
- `PUT /api/emailtemplates/users-reset` (Forgot Password API)

You can also GET, DELETE, or GET `/test` the Email Templates. GET will show you the current contents, while DELETE will delete the template and reset it to the [defaults below](#defaulttemplates). `GET /api/emailtemplates/users-invite/test` will test the current contents of the email template with the current user and some dummy data.

## Templating

The template engine is the same as the rest of the platform: pongo2. More information on this is available in [reactions](/concepts/reactions.md#templates).

The Users-Reset template currently provide access to these context variables:

- User (user being reset)
- URL (generate URL for use in the Meshify Admin Core)
- Token (reset token)
- Theme (theme object for User)

The Users-Invite template currently provide access to these context variables:

- User (user being reset)
- URL (generate URL for use in the Meshify Admin Core)
- Token (reset token)
- Theme (theme object for User)

## Default Templates

#### Invite

```
{{ User.Email }}
{{ User.Information.First }} {{ User.Information.Last }}

Click this link to reset your password and login: {{ URL }}
```

#### Password Reset

```
{{ User.Email }}
{{ User.Information.First }} {{ User.Information.Last }}

Please Change Password. Click the link to reset your password: {{ URL }}
```
