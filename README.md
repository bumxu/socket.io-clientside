
# socket.io-clientside

![NPM version dgdf](https://badge.fury.io/js/socket.io-client.svg)
![NPM version](https://badge.fury.io/js/socket.io-client.svg)

This fork of **socket.io** exposes a clientside-only minimized standalone last release of the **[socket.io-client](https://github.com/socketio/socket.io-client)**.

It allows you to use **npm** to install **socket.io client** in your clientside application downloading and taking solely the space of the main file (~ 70 KB).

## How to use

#### Use it in your HTML documents:

```html
<script src="/node_modules/socket.io-clientside/socket.io.min.js"></script>
<script>
  var socket = io('http://localhost');
  socket.on('connect', function(){});
  socket.on('event', function(data){});
  socket.on('disconnect', function(){});
</script>
```

#### Or require it with [browserify](http://browserify.org/) or your favorite module manager:

```js
var socket = require('socket.io-clientside')('http://localhost');
socket.on('connect', function(){});
socket.on('event', function(data){});
socket.on('disconnect', function(){});
```

## API

### IO(url:String, opts:Object):Socket

  Exposed as the `io` namespace in the standalone build, or the result
  of calling `require('socket.io-clientside')`.

  When called, it creates a new `Manager` for the given URL, and attempts
  to reuse an existing `Manager` for subsequent calls, unless the
  `multiplex` option is passed with `false`.

  The rest of the options are passed to the `Manager` constructor (see below
  for details).

  A `Socket` instance is returned for the namespace specified by the
  pathname in the URL, defaulting to `/`. For example, if the `url` is
  `http://localhost/users`, a transport connection will be established to
  `http://localhost` and a Socket.IO connection will be established to
  `/users`.

### IO#protocol

  Socket.io protocol revision number this client works with.

### IO#Socket

  Reference to the `Socket` constructor.

### IO#Manager

  Reference to the `Manager` constructor.

### IO#Emitter

  Reference to the `Emitter` constructor.

### Manager(url:String, opts:Object)

  A `Manager` represents a connection to a given Socket.IO server. One or
  more `Socket` instances are associated with the manager. The manager
  can be accessed through the `io` property of each `Socket` instance.

  The `opts` are also passed to `engine.io` upon initialization of the
  underlying `Socket`.

  Options:
  - `reconnection` whether to reconnect automatically (`true`)
  - `reconnectionAttempts` (`Infinity`) before giving up
  - `reconnectionDelay` how long to initially wait before attempting a new
    reconnection (`1000`). Affected by +/- `randomizationFactor`,
    for example the default initial delay will be between 500 to 1500ms.
  - `reconnectionDelayMax` maximum amount of time to wait between
    reconnections (`5000`). Each attempt increases the reconnection delay by 2x
    along with a randomization as above
  - `randomizationFactor` (`0.5`), 0 <= randomizationFactor <= 1
  - `timeout` connection timeout before a `connect_error`
    and `connect_timeout` events are emitted (`20000`)
  - `autoConnect` by setting this false, you have to call `manager.open`
    whenever you decide it's appropriate

#### Events

  - `connect_error`. Fired upon a connection error.
    Parameters:
      - `Object` error object
  - `connect_timeout`. Fired upon a connection timeout.
  - `reconnect`. Fired upon a successful reconnection.
    Parameters:
      - `Number` reconnection attempt number
  - `reconnect_attempt`. Fired upon an attempt to reconnect.
  - `reconnecting`. Fired upon an attempt to reconnect.
    Parameters:
      - `Number` reconnection attempt number
  - `reconnect_error`. Fired upon a reconnection attempt error.
    Parameters:
      - `Object` error object
  - `reconnect_failed`. Fired when couldn't reconnect within `reconnectionAttempts`
  - `ping`. Fired when a ping packet is written out to the server.
  - `pong`. Fired when a pong is received from the server.
    Parameters:
      - `Number` number of ms elapsed since `ping` packet (i.e.: latency).

The events above are also emitted on the individual sockets that
reconnect that depend on this `Manager`.

### Manager#reconnection(v:Boolean):Manager

  Sets the `reconnection` option, or returns it if no parameters
  are passed.

### Manager#reconnectionAttempts(v:Boolean):Manager

  Sets the `reconnectionAttempts` option, or returns it if no parameters
  are passed.

### Manager#reconnectionDelay(v:Boolean):Manager

  Sets the `reconectionDelay` option, or returns it if no parameters
  are passed.

### Manager#reconnectionDelayMax(v:Boolean):Manager

  Sets the `reconectionDelayMax` option, or returns it if no parameters
  are passed.

### Manager#timeout(v:Boolean):Manager

  Sets the `timeout` option, or returns it if no parameters
  are passed.

### Socket

#### Socket#id:String

A property on the `socket` instance that is equal to the underlying engine.io socket id.

The property is present once the socket has connected, is removed when the socket disconnects and is updated if the socket reconnects.

#### Socket#compress(v:Boolean):Socket

  Sets a modifier for a subsequent event emission that the event data will
  only be _compressed_ if the value is `true`. Defaults to `true` when you don't call the method.

  ```js
  socket.compress(false).emit('an event', { some: 'data' });
  ```

#### Events

  - `connect`. Fired upon a connection including a successful reconnection.
  - `error`. Fired upon a connection error
    Parameters:
      - `Object` error data
  - `disconnect`. Fired upon a disconnection.
  - `reconnect`. Fired upon a successful reconnection.
    Parameters:
      - `Number` reconnection attempt number
  - `reconnect_attempt`. Fired upon an attempt to reconnect.
  - `reconnecting`. Fired upon an attempt to reconnect.
    Parameters:
      - `Number` reconnection attempt number
  - `reconnect_error`. Fired upon a reconnection attempt error.
    Parameters:
      - `Object` error object
  - `reconnect_failed`. Fired when couldn't reconnect within `reconnectionAttempts`

## License

[MIT](/LICENSE)
