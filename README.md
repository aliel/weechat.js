WeeChat.js
===

[WeeChat Relay Protocol](http://www.weechat.org/files/doc/devel/weechat_relay_protocol.en.html) 
module for [Node.js](http://nodejs.org)

npm
---

    npm install weechat

Usage
---

```JavaScript
var weechat = require('weechat');

// Can only connect to localhost
// First argument is port, second is password
weechat.connect(8000, 'test', function(err) {
    if (!err) {
        console.log('Connected!');

        weechat.write('info version', function(version) {
            console.log('WeeChat version', version);
        });

        weechat.onLine(function(line) {
            var m = weechat.style(line.message);
            console.log('Got line', m);
        });
    }
});
```

Colorizing
---

At the moment the module return only pure WeeChat strings, including coding for colors.   
The module supports stripping these codes away using

```JavaScript
weechat.style(line);
```

Will support real color parsing in the future.  
Note that since a WeeChat string can contain many styles it will be split into 'parts'.


Helper functions
---

Get all buffers including lines:

```JavaScript
weechat.bufferlines(function(buffers) {
    buffers.forEach(function(buffer) {
        buffer.lines = buffer.lines.map(function(line) {
            return {
                prefix: weechat.style(line.prefix),
                message: weechat.style(line.message)
            };
        });

        console.log('Buffer', buffer.id, 'with lines', buffer.lines);
    });
});
```

There are some helper functions for easier listeners

```JavaScript
weechat.onLine(function(line) {
    console.log('Got a line', line);
});

weechat.onOpen(function(buffer) {
    console.log('Buffer opened', buffer);
});

weechat.onClose(function(buffer) {
    console.log('Buffer closed', buffer);
});

weechat.onRenamed(function(buffer) {
    console.log('Buffer renamed', buffer);
});

weechat.onLocalvar(function(lv) {
    console.log('New/changed local variable', lv);
});

weechat.onTitle(function(buffer) {
    console.log('Buffer got new title', buffer);
});

weechat.onNicklist(function(nicklist) {
    console.log('Got nicklist', nicklist);
});
```
