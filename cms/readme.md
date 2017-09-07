# CMS

## Table Of Contents

- [Charting](./chart.md)
- [Templates](./templates.md)
- [Theming](./theming.md)

## Basics

The "CMS" system is a very light CMS system that allows customers to modify
and tweak the different templates, colors, logos, and other such things to adjust
a Meshify UI to their liking.

It consists of the following entities:

- Domain
- Application
- Core

### Domain

A Domain is the entrypoint to your UI on Meshify. Once you setup a Domain, that Domain is only accessible
to you as a customer. A Domain has a Theme object to provide [Theming](./theming.md), a list of Applications
accessible at that Domain, and a default Application to route to if no path is provided (i.e. 
https://carbon.meshify.com will redirect to https://carbon.meshify.com/admin).

### Application

An Application is an object that allows you to add a basePath to a Core. Depending on which Domains point to an
Application, this allows you to make a Core available at a particular basePath on a Domain. For example if you had:

```
Cores:
id=1, name=Admin
id=2, name=Dashboard

Applications:
id=1, name=AdminAtHome, basePath=home
id=2, name=AdminAtSettings, basePath=settings
id=3, name=Dashboard, basePath=dashboard

Domains:
id=1, fqdn: somedomain.meshify.com, applications=[1,2,3], defaultApplication=1
id=2, fqdn: otherdomain.meshify.com, applications=[2,3], defaultApplication=3
```

Here is how your routing would work out:

- route to: https://somedomain.meshify.com > redirect to https://somedomain.meshify.com/home
- route to: https://somedomain.meshify.com/home > receive the index.html for the Admin Core
- route to: https://somedomain.meshify.com/dashboard > receive the index.html for the Dashboard Core
- route to: https://somedomain.meshify.com/settings > receive the index.html for the Admin Core
- route to: https://otherdomain.meshify.com > redirect to https://otherdomain.meshify.com/dashboard
- route to: https://otherdomain.meshify.com/home > receive a 404 Not Found
- route to: https://otherdomain.meshify.com/dashboard > receive the index.html for the Dashboard Core
- route to: https://otherdomain.meshify.com/settings > receive the index.html for the Admin Core

### Core

A Core is the heart of a UI application. It consists of files in a folder and works as a [SPA application](https://en.wikipedia.org/wiki/Single-page_application) with a single index.html.

A Core enables Theming through its own Javascript code, and Core files are what the server delivers to you when 
you go to a Meshify Site in the browser (i.e. https://carbon.meshify.com).