# Create and Test a Reaction

## Overview
In this example we will run through all of the steps to create and test a reaction.  Specifically, we will be creating a dummy node, node type, rule, reaction, and notification schema that will email a user when it receives a signal.  Then we will subscribe to it and test the reaction. This example assumes you already have a login to the Carbon Admin.

## Steps
First you must have a working login to the Carbon Admin.  Log into Carbon.

### Create a user
If you don't already have a user with a working email, you will need to create a user with a working email address that you can access.  For simplicity we will choose an admin user with access to all folders.
- Select "Users" from the left navigation under "Access".
- Select "Add User" near the upper right corner.
- Give your user a First Name, Last Name, and an Email Address.
- Select a Role, in this example we will use "admin".
- Check the "All Folders" check-box under "Folders".
- Click "Save and Invite" at the bottom of the page.
- Check your email and follow the instructions on the invite email.

### Create a Node Type
Node types control what channels are in a set of nodes.  How a node behaves is based on what node type it is.  See [Nodes and Node Types](concepts/nodes.md) for more details.
- From the navigation on the left, under "IOT", select "Node Types".
- Select "Add Node Type" near the upper right corner.
- Enter a Name, this must only contain alpha-numeric characters and the dash (-), no spaces. Example: `My-Dummy-Node-Type`
- Enter a Vanity Name, this is a much more flexible string, and does not have the same restrictions as the Name field. Example: "My Dummy Node Type"
- Check the "All Roles" check-box under "Allowed Roles".
- Create at least one channel, click the "Add Channel" button under the list of roles.  
- The "Name" and "Vanity Name" rules are similar to the Node Type "Name" and "Vanity Name" rules.  For this example, we will use "dummychan" for the Name, and "Dummy Channel" for the Vanity Name.  Then select "boolean" for the type, "false" for the default value, and you can ignore the rest.
- Click the "Save" button at the bottom of the page.

### Create a Node
Nodes represent an individual device which has a Node Type to define its channels, rules, and behaviors. For more details see [Nodes and Node Types](concepts/nodes.md).
- Select "Nodes" from the left navigation under "IOT".
- Click the "Add Node" button near the upper right corner.
- Give the node a Vanity Name, such as "My Dummy Node".
- Give the node a Unique ID, such as "dummyabc123", this should be unique to this node only.
- Under settings, choose the node type you created earlier (My Dummy Node Type).
- Do not select a parent node, that will make your new node have itself as a parent.
- Choose a folder for the node, make sure it is a folder you have access to.
- ***IMPORTANT*** Check the "Active" check-box under the node type listing.
- Click the "Save" button at the bottom of the page.

### Create a Rule
Carbon rules allow basic on/off for a node state. All rule conditions must return a single value: true or false.  For more details, see [Rules](concepts/rules.md).
- Select "Rules" from the left navigation under "Alarms"
- Select "Add Rule" near the upper right corner.
- Give the rule a name. Example: "My Dummy Rule"
- Under "Definitions", choose the node type you created earlier (My Dummy Node Type).
- Under "Channel Name" select the channel name you created in your node type earlier (dummychan).
- Under "Condition", write `dummychan.value == true`.
- Check the "Always React" check-box.  If you do not check this, the reaction will only fire when the node enters the "true" state, if you do check this, it will react with every signal from a node.
- Click the "Save" button at the bottom of the page.

### Create a Reaction
Reactions are the result of a state change from a rule. For more details, see [Reactions](concepts/reactions.md).
- Select "Reactions" from the left navigation under "Alarms".
- Select "Add Reaction" near the upper right corner.
- Give the reaction a Name. Example "My Dummy Reaction"
- ***IMPORTANT*** Check the "Active" check-box.
- Select the rule you created above (My Dummy Rule).
- Under "Event Type" select "enter".
- Under "Reaction Type" select "email".
- Under "Subject Template" write "My Dummy Reaction Email".
- Under "Body Template" write "My reaction has fired!".  For more details on what is possible here, see [Email Templates](concepts/emailtemplates.md).
- Click the "Save" button at the bottom of the page.

### Create a Notification Schema
Notification Schemas are schedules controlling when a particular reaction can fire.
- Select "Notification Schemas" from the left navigation under "Notifications".
- Give your Notification Schema a name.  Example: "My Dummy Notification Schema"
- Under "Reactions" select the reaction you created above (My Dummy Reaction).
- Click the "Add Schedule" button under the "Default Schedule".
- By default it should come up with a schedule that is 24/7, this is fine for this example.
- Click the "Save" button at the bottom of the page.

### Subscribe to the Notification Schema
To receive reactions, you will need to subscribe to the Notification Schema you just created.
- Select "Subscriptions" from the left navigation under "Notifications".
- Find your user or the one you created above in the list and click on it.
- You should now be at a listing of all schemas available to your user.  Find the schema you created above (My Dummy Notification Schema") and check the box next to it.
- Click the "Save" button at the bottom of the page.

### Test the Reaction
Congratulations, you have set up everything, you now can test your work.  The main admin does not have the ability to simulate node reactions. For this we will be directly accessing the Carbon API using [curl](https://curl.haxx.se/), but you can achieve similar results with something like [Postman](https://www.postman.com/).
- For this we will need some information about the things we created above:
  - The Node ID, you can get this by editing the node in the admin, look at the URL and find the number after "nodes/": https://carbon.meshify.com/admin#/nodes/***181359***?pageNumber=0&rowsPerPage=50&tab=0.  In this case, 181359.
  - The channel name: "dummychan"
  - A time from within the past 24 hours in UTC format like so `2020-02-19T17:08:04Z`. You can get the current UTC time at [https://www.timeanddate.com/worldclock/timezone/utc](https://www.timeanddate.com/worldclock/timezone/utc).
  - Your login credentials to the Carbon Admin.
- POST to the publish API to simulate the node firing a message.  Replace `<<NODE_ID>>` in both spots with the node id you found above.  Replace `<<TIMESTAMP>>` with the time from above in the format `2020-02-19T17:08:04Z`, `<<USERNAME>>` with your Carbon User Name, `<<PASSWORD>>` with your Carbon Password:
  ```
  curl -X POST -i -d '{"channelName":"dummychan","nodeId":<<NODE_ID>>,"parentNodeId":<<NODE_ID>>,"timestamp":"<<TIMESTAMP>>","traceId":"hn8g7gvpz3","value":true}' -u "<<USERNAME>>:<<PASSWORD>>" https://carbon.meshify.com/api/publish/cloud
  ```
 - Wait a minute and check your email, you should have received an email with the reaction created above!
