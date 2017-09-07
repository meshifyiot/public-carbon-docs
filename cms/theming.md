# Theming

For general customization, such as:

- changing colors
- providing different home pages
- changing logos and images
- altering text

We provide a generic Theme object (much like the metadata object) attached to
Domains and Folders.

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

Once there is an authenticated User, we can grab the Theme based on the current users Folder context. This works
by taking the original Domain.Theme and overwriting any keys within that by going down the entire Folder chain.
So if there was a Domain with Theme:

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
For A:
{
    "logoUrl": "https://domain.com/images/logo.png"
}

For B:
{
    "titleText": "Company B Core"
}

For C:
{
    "homePage": "users"
}
```

And we were getting the Theme for an User in Folder C, we would receive:

```
{
    "logoUrl": "https://domain.com/images/logo.png",
    "titleText": "Company B Core",
    "homePage": "users",
    "showButton": false
}
```

