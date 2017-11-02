# Meshify Carbon

### Table of Contents
- [Things](#Things)
- [Visualization](#visualization)
- [People](#people)
- [Hierarchy](#hierarchy)
- [Notifications](#notifications)
- Web App
- Content Management
- [Extensibility](#extensibility)

<!-- ![Carbon Data Model](img/carbon_data_model.png) -->


## <a name="Things">Things</a>

### <a name="nodes">Nodes</a>

In Meshify, each connected thing (sensor, equipment, gateway, etc.) is a node. A node is a collection of data, which are organized on [channels](#channels).

Each node has a [node type](#node-types). The node type defines the channels, html templates, lambda functions, and drivers.

Each node is placed within a [folder](#folders), which orgainizes people and nodes within the hierarchy.

Nodes have [Metadata](#metadata).

### <a name="node-types">Node Types</a>

The node type defines the templatized configuration for a node. It can also be thought of as a class, where the node is an instance, or object of the class. Each of the following are configured on a node type, which are then used by every node of that node type.

* [Channels](#channels)
* [HTML Templates](#html-templates)
* [Notification Rules](#rules)
* [Lambda Functions](#lambda-functions)
* [Drivers](#drivers)

A node type is the primary way that nodes receive customization. However node types can be built to allow node-specific customization. For example, a notification rule threshold based on a channel value that is defined different for each node.

Node types are important because they lock together a data structure (channels) with features that use the data structure (such as html templates, notifications, and lambda functions).

### <a name="channels">Channels</a>

Channels store all timestamped values for nodes. They are like data fields or columns in a database. They provide context through naming and are used to reference node data.

Each channel has a defined data type: `String`, `Number`, or `Boolean`.

### <a name="alias-channels">Alias Channels</a>

Alias channels provide a way of forwarding data from one channel to another channel. Data remains at the original channel and is also copied to another designated channel. 

## <a name="visualization">Visualization</a>

### <a name="html-templates">HTML Templates</a>

HTML Templates define how live data is visualized in Meshify. They are fully customizable HTML and can leverage some of our built-in components, such as line graphs.

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

Folders are the basic, recursive unit of hierarchy. A user's location(s) within a hierarchy dictates which nodes are visibile to that user.

A user can see nodes at or below their assigned folder(s), but cannot see any folders above them. They can also see nodes within any folders that are contained by their folder(s).

A single folder sits at the top of the hierarchy. A user placed within this folder has access to the entire system.

Folders have [Metadata](#metadata).

## <a name="notifications">Notifications</a>

The life cycle of a notification relies on several objects:

1. Rule: A rule condition is triggered.
2. Icon: Each rule is given a severity level, indicated by the icon
2. Reaction: The rule's reaction uses a template to generate an alert message
3. Subscription + Schema: The subscription ties a user to notification schema which indicates the notification schedule

### <a name="rules">Rules</a>

See [rules documentation](rules.md) documentation.

### <a name="reactions">Reactions</a>

See [reactions documentation](reactions.md).

### <a name="icons">Icons</a>

See [icons documentation](icons.md) documentation.

### <a name="notification-schema">Notification Schema</a>

See [schedules documentation](schedules.md) documentation.

### <a name="subscriptions">Subscriptions</a>

Subscription links a user with a notification schema, and subscribes the user to related reactions.

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
### <a name="lambda-functions">Lambda Functions</a>

Lambda functions are javascript functions that run when a value is received on a designated channel. They are used to transform incoming data or run sophisticated rules algorithms. A built-in library provides commonly used functionlaity such as retrieving current values from other channels, getting historical data, or sending values to other channels. Lambdas are short-lived (typically <10ms) and do not retain state (but clever lambda developers may use channels to perpetuate state).

Common uses of Lambdas include:

1. Forwarding data from one channel to another
1. Streaming calculations, such as calculating a channel's deriverative
1. Rules processing that are based on history or channels on another node

### <a name="metadata">Metadata</a>

Instance objects in Carbon (such as users and folders) have json-based metadata container that can be used to extend the data structure for nodes. Metadata can be read and written from within templates on the standard dashboard core or used with new applications or cores.


### <a name="drivers">Drivers</a>

Drivers are files that physical devices can download and run. If a node's node type changes, it will download the driver associated with that node type and change functionality.

Drivers also include code which instruct devices to send data to meshify on channels that are defined by the node type.
