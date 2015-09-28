# websocket-stream

## Forker notes

I(jacobbubu) made this fork beacuse WebSocket polyfill in react native does not implement EventTarget interface(https://github.com/facebook/react-native/issues/2583).

I've just changed 4 lines in [stream.js](https://github.com/jacobbubu/rn-websocket-stream/blob/master/stream.js#L26-L35) to

```
  if (socket.readyState === 1) {
    stream = proxy
  } else {
    stream = duplexify.obj()
    // 1 changed here
    socket.onopen = onready
  }

  // changed 3 lines
  socket.onclose = onclose
  socket.onerror = onerror
  socket.onmessage = onmessage
```


[![NPM](https://nodei.co/npm/websocket-stream.png?global=true)](https://nodei.co/npm/websocket-stream/)

Use HTML5 [websockets](https://developer.mozilla.org/en-US/docs/WebSockets) using the Node Streams API.

### Usage

This module works in Node or in Browsers that support WebSockets. You can use [browserify](http://github.com/substack/node-browserify) to package this module for browser use.

```javascript
var websocket = require('websocket-stream')
var ws = websocket('ws://realtimecats.com')
ws.pipe(somewhereAwesome)
```

In the example above `ws` is a duplex stream. That means you can pipe output to anything that accepts streams. You can also pipe data into streams (such as a webcam feed or audio data).

The underlying `WebSocket` instance is available as `ws.socket`.

### On the server

Using the [`ws`](http://npmjs.org/ws) module you can make a websocket server and use this module to get websocket streams on the server:

```javascript
var websocket = require('websocket-stream')
var wss = websocket.createServer({server: someHTTPServer}, handle)

function handle(stream) {
  fs.createReadStream('bigdata.json').pipe(stream)
})
```

## Run the tests

### Server-side tests

```
npm test
```

### Client-side tests

First start the echo server by running `node test-server.js`

Then run `npm start` and open `localhost:9966` in your browser and open the Dev Tools console to see test output.

## license

BSD LICENSE
