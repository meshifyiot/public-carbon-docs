# Email Templates

## Basics

Email Templates are templates used for emails sent by the Meshify Platform during certain events. They are in no way related to Alarms, Reactions, or Notifications.

Currently there are two events which make use of Email Templates:

- User Invite (triggered by `POST /api/authentication/membership`)
- Password Reset (triggered by `/api/passwords/resetpassword?email=someemail@somewhere.com`)

If the user doesn't have any overrides specified, the [defaults](#default-templates) will be used. You can create a new set of overrides via `POST /api/emailtemplates/` with the key specified in the body and related fields except for the body. To Modify the body of the template you must use a `PUT /api/emailtemplates/{key}/source` where `{key}` is either `users-invite` or `users-reset`. 

You can also GET, DELETE, or GET `/test` the Email Templates. GET will show you the current contents, while DELETE will delete the template and reset it to the [defaults below](#default-templates). `GET /api/emailtemplates/users-invite/test` will test the current contents of the email template with the current user and some dummy data.

## Templating

The template engine is the same as the rest of the platform: pongo2. More information on this is available in [reactions](/concepts/reactions.md#templates). In addition to the email body, the from name, from email and subject fields are also templateable. 

The Users-Reset template currently provide access to these context variables:

- User (user being reset)
- URL (generate URL for use in the Meshify Admin Core)
- Token (reset token)
- Theme (theme object for User)
- Protocol (http or https)
- Domain (i.e. carbon.meshify.com)
- Folders (the folders in the current scope and their related meta data)

The Users-Invite template currently provide access to these context variables:

- User (user being reset)
- URL (generate URL for use in the Meshify Admin Core)
- Token (reset token)
- Theme (theme object for User)
- Protocol (http or https)
- Domain (i.e. carbon.meshify.com)
- Folders (the folders in the current scope and their related meta data)

## Default Templates

#### Invite

##### Subject
```
Welcome To Carbon!
```
##### From Name
```
meshify
```
##### From Email 
```
noreply@meshify.com
```
##### Body
```
{{ User.Email }}
{{ User.Information.First }} {{ User.Information.Last }}

Click this link to reset your password and login: {{ URL }}
```

#### Password Reset

##### Subject
```
Meshify Carbon: Reset Password Link
```
##### From Name
```
meshify
```
##### From Email 
```
noreply@meshify.com
```
##### Body
```
{{ User.Email }}
{{ User.Information.First }} {{ User.Information.Last }}

Please Change Password. Click the link to reset your password: {{ URL }}
```
