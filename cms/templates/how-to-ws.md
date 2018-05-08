## Node Type Templates: DIY Websocketing

### Introduction

Websocketing is easy. Look below for a code example of how mi-channel-value is created.

## Syntax

```
<sample-template>

    <span class='mv0'>{value}</span>

    <script>
        var tag = this;
        tag.opts.node = tag.parent.opts.node
        tag.mixin('websocket');

        tag.value = tag.opts.node.channels[tag.opts.channel].value ? tag.opts.node.channels[tag.opts.channel].value : '--';
        if(typeof tag.value === 'number'){
            tag.value = parseFloat(tag.value.toFixed(2))
        }
        tag.subscribe(tag.opts.node.id, function(msg) {
            if(msg.channelName === tag.opts.channel) {
                tag.value = msg.value
                tag.update();
            }
        });
    </script>

</sample-template>

```

Notes
---

Step 1. You start off be defining the context of the tag with var tag = this. 

Step 2. Invoke tag.mixin('websocket') to import the subscribe function to your tag.

Step 3. On your tag.subscribe you absolutely need the nodeId to subscribe to any websocket updates on that node.

Step 4. Make use of the mi-channel-debug to publish data to the cloud and trigger websocket updates on this current node.

Step 5. Inside your subscribe function you need logic to run if and only if the message.channelName matches the channel you are listening for. If you are confused then console.log(msg) and the channel you are looking for to see what is matching up or not.

Step 6. Always make sure to run tag.update() so the changes you make trigger a re-render of the component.

 
