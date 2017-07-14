# Meshify Carbon

### Table of Contents
- [Things](#Things)
- [Visualization](#visualization)
- [People](#people)
- [Hierarchy](#hierarchy)
- Notifications
- Web App
- Content Management
- [Extensibility](#extensibility)

<!-- ![Carbon Data Model](img/carbon_data_model.png) -->


## <a name="Things">Things</a>

### <a name="nodes">Nodes</a>

In Meshify, each connected thing (sensor, equipment, gateway, etc.) is a node. A node is a collection of data, which are organized on [channels](#channels).

Each node has a [node type](#node-types). The node type defines the channels, html templates, transforms, and drivers.

Each node is assigned to a [site](#sites) and the site is assigned to a [folder](#folders) within the hierarchy. Through the site assignment, a node adopts those folder permissions.

Nodes have [Metadata](#metadata).

### <a name="node-types">Node Types</a>

The node type defines the templatized configuration for a node. It can also be thought of as a class, where the node is an instance, or object of the class. Each of the following are configured on a node type, which are then used by every node of that node type.

* [Channels](#channels)
* [HTML Templates](#html-templates)
* [Notification Rules](#rules)
* [Transforms](#transforms)
* [Drivers](#drivers)

A node type is the primary way that nodes receive customization. However node types can be built to allow node-specific customization. For example, a notification rule threshold based on a channel value that is defined different for each node.

Node types are important because they lock together a data structure (channels) with features that use the data structure (such as html templates, notifications, and transforms).


### <a name="channels">Channels</a>

Channels store all timestamped values for nodes. They are like data fields or columns in a database. They provide context through naming and are used to reference node data.

Each channel has a defined data type: `String`, `Number`, or `Boolean`.


### <a name="sites">Sites</a>

In Meshify, all nodes are contained within sites. The site has a location. A site has a status based on the status and roll-up settings of contained nodes.

Sites have [Metadata](#metadata).

## <a name="visualization">Visualization</a>

### <a name="html-templates">HTML Templates</a>

HTML Templates provide a customized and dynamic view of data from a particular node.

Each html template is a [Riot.js](http://riotjs.com) custom tag. Templates use Meshify library functions and Meshify custom tags to visualize channel data numerically or in charts.

HTML Templates leverage [Tachyons](http://tachyons.io) CSS Toolkit and [Moment.js](http://momentjs.com).

```
When finalized, html snippets will go here
```

### <a name="site-templates">Site Templates</a>

Site templates are like node HTML Templates, but they are associated with [sites](#sites).

## <a name="people">People</a>

### <a name="users">Users</a>

Each person accessing Meshify is a user. A user authenticates using their e-mail address and password.

A user is placed in one or more folders and has access to any folders at or below their assignment.

Users have [Metadata](#metadata).

### <a name="roles">Roles</a>

Meshify roles are a set of permissions that dictate the level of access for users.

### <a name="guests">Guests</a>

Guests are people that do not log into Meshify, but have [subscriptions](#subscriptions) to [notification schema](#notification-schema).

## <a name="hierarchy">Hierarchy</a>

### <a name="folders">Folders</a>

Folders are the basic, recursive unit of hierarchy. A user's location(s) within a hierarchy dictates which sites (and therefore which nodes) are visibile to that user.

A user can see sites at or below their assigned folder(s), but cannot see any folders above them. Said another way, a user can see sites within their folder and any folders contained by their folder, but cannot see contain_ing_ folders.

A single folder sits at the top of the hierarchy. A user placed within this folder may see the contents of any folder in the system.

Folders have [Metadata](#metadata).


## Notifications

### <a name="rules">Rules</a>

### <a name="reactions">Reactions</a>

### <a name="icons">Icons</a>

### <a name="notification-schema">Notification Schema</a>

### <a name="subscriptions">Subscriptions</a>

## Web App

### <a name="tenant">Tenant</a>

### <a name="themes">Themes</a>

### <a name="domains">Domains</a>

### <a name="applications">Applications</a>

### <a name="cores">Cores</a>

## Content Management
### <a name="provisioning-files">Provisioning Files</a>

### <a name="files">Files</a>

## <a name="extensibility">Extensibility</a>
### <a name="transforms">Transforms</a>

Transforms (aka lambda functions) are javascript functions that run when a value is received on a designated channel. They are used to transform incoming data or run sophisticated rules algorithms. A built-in library provides commonly used functionlaity such as retrieving current values from other channels, getting historical data, or sending values to other channels. Lambdas are short-lived (typically <10ms) and do not retain state.

Common uses of Lambdas include:

1. Forwarding data from one channel to another
1. Streaming calculations, such as calculating a channel's deriverative
1. Rules processing that are based on history or channels on another node

### <a name="metadata">Metadata</a>

Instance objects in Carbon (such as users and folders) have json-based metadata container that can be used to extend the data structure for nodes. Metadata can be read and written from within templates on the standard dashboard core or used with new applications or cores.


### <a name="drivers">Drivers</a>

Drivers are files that physical devices can download and run. If a node's node type changes, it will download the driver associated with that node type and change functionality.

Drivers also include code which instruct devices to send data to meshify on channels that are defined by the node type.
