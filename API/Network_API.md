# Tessel 2 Network API

* [Wifi](#wifi)
* [Access Point](#access-point)

When you `require('tessel')` within a script which is executed on Tessel 2, this loads a library which interfaces with the Tessel 2 hardware, including the wireless radios. The code for Tessel 2's hardware object can be found [here](https://github.com/tessel/t2-firmware/blob/master/node/tessel-export.js).

While [the cli](https://tessel.io/docs/cli#usage) can be used to configure the Tessel to connect to local wifi networks and to create a custom wireless network, the `tessel` package, via the Network API, can be used in the same way. This functionality can be useful for programs that want to create on-demand access points or connect to a local network chosen by someone interacting with a web app served by a Tessel. Having some control of the networking abilities make Tessel a powerful, web-enabled device.

## Wifi

The following methods and properties are available through `tessel.network.wifi`.

### Methods

- [`connect`](#connect)
- [`connection`](#connection)
- [`disable`](#disable)
- [`enable`](#enable)
- [`findAvailableNetworks`](#findAvailableNetworks)
- [`reset`](#reset)

### connect

Attempts to connect to a local wifi network using the passed-in configuration object. 

```js
tessel.network.wifi.connect(config, callback);
```

| argument | type   | description   | required |
|:---------|:-------|:--------------|:---------|
| config   | Object | { ssid, password, security } configuration used to connect to local network | yes |
| callback | Function | called when done connecting or if an error occurred | no |

#### Example

```js
tessel.network.wifi.connect({
  ssid: 'WifiNetworkName', // required
  password: 'WifiNetworkPassword', // required if network is password-protected
  security: 'psk2' // available values - none, wep, psk, psk2, wpa, wpa2, default is 'none' if no password needed, default is 'psk2' otherwise. See https://tessel.io/docs/cli#usage for more info
}, function (error, settings) {
  if (error) {
    // handle error if it exists
  }

  console.log(settings); // object containing ssid, security, IP address
});
```

### connection

Returns the settings for the current connection, includes ssid (network name), security, and IP address.

```js
tessel.network.wifi.connection(callback);
```

#### Example

```js
tessel.network.wifi.connection(function(error, settings) {
  if (error) {
    throw error;
  }

  console.log(settings.ssid); // logs the name of the wifi network being used by Tessel
});
```

### disable

Disables Tessel's connection to wifi.

```js
tessel.network.wifi.disable(callback);
```

| argument | type   | description   | required |
|:---------|:-------|:--------------|:---------|
| callback | Function | called when done disabling or if an error occurred | no |

#### Example

```js
tessel.network.wifi.disable(function (error) {
  if (error) {
    // handle error if it exists
  }

  // Tessel is now disconnected from wifi
});
```

### enable

Enables Tessel's connection to wifi.

```js
tessel.network.wifi.enable(callback);
```

| argument | type   | description   | required |
|:---------|:-------|:--------------|:---------|
| callback | Function | called when done enabling or if an error occurred | no |

#### Example

```js
tessel.network.wifi.enable(function (error) {
  if (error) {
    // handle error if it exists
  }

  // Tessel is now connected to wifi
});
```

### findAvailableNetworks

Scans for local wifi networks available to Tessel.

```js
tessel.network.wifi.findAvailableNetworks(callback);
```

| argument | type   | description   | required |
|:---------|:-------|:--------------|:---------|
| callback | Function | called when done scanning or if an error occurred | no |

#### Example

```js
tessel.network.wifi.findAvailableNetworks(function (error, networks) {
  if (error) {
    // handle error if it exists
  }

  console.log(networks) // 'networks' is an array of objects containing { ssid, security, quality }, quality is a string comparing the signal strength out of 70 - '49/70'
});
```

### reset

Resets the Tessel's wifi connection, first disabling, then enabling the connection.

```js
tessel.network.wifi.reset();
```

| argument | type   | description   | required |
|:---------|:-------|:--------------|:---------|
| callback | Function | called when Tessel is done resetting | no |

#### Example

```js
tessel.network.wifi.reset(function (error) {
  if (error) {
    // handle error if it exists
  }

  // Tessel's wifi connection has been successfully reset
});
```

### Properties

- [`isConnected`](#isConnected)

### isConnected

The state of Tessel's connection to wifi, returns true or false.

```js
tessel.network.wifi.isConnected;
```

#### Example

```js
if (tessel.network.wifi.isConnected) {
  // Tessel is connected to wifi
} else {
  // Tessel is not connected to wifi
}
```

### Events

The `WifI` Class inherits from the Node.js [`EventEmitter`](https://nodejs.org/api/events.html#events_class_eventemitter) Class, so methods such as [`on`](https://nodejs.org/api/events.html#events_emitter_on_eventname_listener), [`once`](https://nodejs.org/api/events.html#events_emitter_once_eventname_listener), and [`removeListener`](https://nodejs.org/api/events.html#events_emitter_removelistener_eventname_listener) can be used with the following events:

- `connect`: this event is emitted when Tessel has successfully **connected** to a wifi network
- `disconnect`: this event is emitted when Tessel has successfully **disconnected** from a wifi network
- `error`: this event is emitted when an error occurs during any of the wifi functions

#### Example

```js
tessel.network.wifi.on('connect', function (settings) {
  // the 'connect' event has been called and happens to return a 'settings' object
});
```


## Access Point

The following methods and properties are available through `tessel.network.ap`.

### Methods

- [`create`](#create)
- [`disable`](#ap-disable)
- [`enable`](#ap-enable)
- [`reset`](#ap-reset)

### create

Attempts to create a custom access point using the passed-in configuration object. 

```js
tessel.network.ap.create(config, callback);
```

| argument | type   | description   | required |
|:---------|:-------|:--------------|:---------|
| config   | Object | { ssid, password, security } configuration used to create a custom access point | yes |
| callback | Function | called when done creating or if an error occurred | no |

#### Example

```js
tessel.network.ap.create({
  ssid: 'AccessPointName', // required
  password: 'CustomSecurePassword', // required if network is password-protected
  security: 'psk2' // available values - none, wep, psk, psk2, default is 'none' if no password needed, default is 'psk2' otherwise. See https://tessel.io/docs/cli#usage for more info
}, function (error, settings) {
  if (error) {
    // handle error if it exists
  }

  console.log(settings); // object containing ssid, security, IP address
});
```

### disable

Disables the created access point.

```js
tessel.network.ap.disable(callback);
```

| argument | type   | description   | required |
|:---------|:-------|:--------------|:---------|
| callback | Function | called when done disabling or if an error occurred | no |

#### Example

```js
tessel.network.ap.disable(function (error) {
  if (error) {
    // handle error if it exists
  }

  // the access point has been successfully disabled
});
```

### enable

Enables a previously configured access point.

```js
tessel.network.ap.enable(callback);
```

| argument | type   | description   | required |
|:---------|:-------|:--------------|:---------|
| callback | Function | called when done enabling or if an error occurred | no |

#### Example

```js
tessel.network.ap.enable(function (error) {
  if (error) {
    // handle error if it exists
  }

  // the access point has been successfully enabled
});
```

### reset

Resets the active access point, first disabling, then enabling the network.

```js
tessel.network.ap.reset();
```

| argument | type   | description   | required |
|:---------|:-------|:--------------|:---------|
| callback | Function | called when Tessel is done resetting | no |

#### Example

```js
tessel.network.ap.reset(function (error) {
  if (error) {
    // handle error if it exists
  }

  // Tessel's wifi connection has been successfully reset
});
```

### Events

The `AP` Class inherits from the Node.js [`EventEmitter`](https://nodejs.org/api/events.html#events_class_eventemitter) Class, so methods such as [`on`](https://nodejs.org/api/events.html#events_emitter_on_eventname_listener), [`once`](https://nodejs.org/api/events.html#events_emitter_once_eventname_listener), and [`removeListener`](https://nodejs.org/api/events.html#events_emitter_removelistener_eventname_listener) can be used with the following events:

- `create`: this event is emitted when an access point is successfully created, includes `settings` object in the callback
- `disable`: this event is emitted when an access point is successfully disabled
- `error`: this event is emitted when an error occurs during any of the access point functions
- `enable`: this event is emitted when an access point is successfully enabled
- `reset`: this event is emitted when an access point is preparing to reset

#### Example

```js
tessel.network.ap.on('create', function (settings) {
  // the 'create' event has been called and happens to return a 'settings' object
});
```
