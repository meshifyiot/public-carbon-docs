# Theming

For general customization, such as:

- changing colors
- providing different home pages
- changing logos and images
- altering text

We provide a generic Theme object (much like the metadata object) attached to
Domains, Applications, and Folders.

Values provided in this object need to be specifically requested and used within each
Core for them to have any effect.

### How to Retrieve Themes

The reason that themes are provided on both the Domain and Folder objects is that we would
like to be able to apply themes to a Core when there is no authenticated User (i.e. there's no
folder context yet in which to get the theme).

To do this, we run the index.html of each Core through a template generator (much like all of our 
email templates). The easiest way to insert the Theme into the current javascript context is to add
a script tag to the index.html like so: 

```
<script type="text/javascript">
    window.BasePath = "{{ BasePath }}";
    window.Theme = {{ ThemeJSON | safe }};
</script>
```

This will add the current BasePath to the window, as well as adding the theme object in Javascript syntax
to the window. This will allow you to use any values within the theme to apply changes in the Core.

The theme added in this manner will take on the Domain.Theme overwitten by the Application.Theme (based on the path).

Once there is an authenticated User, we can grab the Theme based on the current user's Folder context. This works
by taking the root Folder.Theme and overwriting any keys within that by going down the entire Folder chain.
So if there was a Folder `A` with Theme:

```
{
    "logoUrl": "/images/logo.png",
    "titleText": "Meshify Core",
    "homePage": "roles",
    "showButton": false
}
```

And we had the folder structure: 

```
A
|_ 
  B
  |_
    C
|_
  D
```

And we had the following themes:

```
For B:
{
    "logoUrl": "https://domain.com/images/logo.png",
    "titleText": "Company B Core"
}

For C:
{
    "homePage": "users"
}
```

And we were getting the Theme for a User in Folder C, we would receive:

```
{
    "logoUrl": "https://domain.com/images/logo.png",
    "titleText": "Company B Core",
    "homePage": "users",
    "showButton": false
}
```

### Sharing the Theme Object

An interesting issue that comes about when using the Theme object across multiple cores
is that one needs to pay particular attention to what they want to share and what they want to
separate.

Our general recommendation is that each core have its own "keyspace". So if you have 3 cores:
Dashboard, Admin, and Activate; we would recommend a Theme JSON structure such as:

```
{
    "admin": {
        "key": "value"
    },
    "dashboard": {
        "key": "value"
    },
    "activate": {
        "key": "value"
    }
}
```

### List of Theme Properties

This is an ongoing list of theme properties that will change the look and behavior of Admin and Dashboard by default. Please note that theme key/value pairs can be referenced in nodetype templates, so there is essentially no limit to the amount of properties that can be leveraged.

- logoUrl: file path to an image that will be used as the company's logo
- titleText: Text to be displayed in the browser tab while the property is open
- primaryColor: Main color to be displayed in the Dashboard core
