# jest-dev-server

Starts a server before your Jest tests and tears it down after.

## Why

`jest-puppeteer` works great for running tests in Jest using Puppeteer.
It's also useful for starting a local development server during the tests without letting Jest hang.
This package extracts just the local development server spawning without any ties to Puppeteer.

## Usage

`jest-dev-server` exports `setup` and `teardown` functions.

```js
// global-setup.js
const { setup: setupDevServer } = require('jest-dev-server')

module.exports = async function globalSetup() {
  await setupDevServer({
    command: `node config/start.js --port=3000`,
    launchTimeout: 50000,
    port: 3000,
  })
  // Your global setup
}
```

```js
// global-teardown.js
const { setup: teardownDevServer } = require('jest-dev-server')

module.exports = async function globalTeardown() {
  await teardownDevServer()
  // Your global teardown
}
```

#### Options

#### `command`

Type: `string`, required.

Command to execute to start the port.
Directly passed to [`spawnd`](https://www.npmjs.com/package/spawnd).

```js
module.exports = {
  command: 'npm run start',
}
```

#### `debug`

Type: `boolean`, default to `false`.

Log server output, useful if server is crashing at start.

```js
module.exports = {
  command: 'npm run start',
  debug: true,
}
```

#### `launchTimeout`

Type: `number`, default to `5000`.

How many milliseconds to wait for the spawned server to be available before giving up.
Defaults to [`wait-port`](https://www.npmjs.com/package/wait-port)'s default.

```js
module.exports = {
  command: 'npm run start',
  launchTimeout: 30000,
}
```

#### `options`

Type: `object`, default to `{}`.

Any other options to pass to [`spawnd`](https://www.npmjs.com/package/spawnd).

#### `port`

Type: `number`, default to `null`.

Port to wait for activity on before considering the server running.
If not provided, the server is assumed to immediately be running.

```js
module.exports = {
  command: 'npm run start --port 3000',
  port: 3000,
}
```

#### `usedPortAction`

Type: `string` (`ask`, `error`, `ignore`, `kill`) default to `ask`.

It defines the action to take if port is already used:

- `ask`: a prompt is shown to decide if you want to kill the process or not
- `error`: an errow is thrown
- `ignore`: your test are executed, we assume that the server is already started
- `kill`: the process is automatically killed without a prompt

```js
module.exports = {
  command: 'npm run start --port 3000',
  port: 3000,
  usedPortAction: 'kill',
}
```

## License

MIT

[build-badge]: https://img.shields.io/travis/smooth-code/jest-puppeteer.svg?style=flat-square
[build]: https://travis-ci.org/smooth-code/jest-puppeteer
[version-badge]: https://img.shields.io/npm/v/jest-dev-server.svg?style=flat-square
[package]: https://www.npmjs.com/package/jest-dev-server
[license-badge]: https://img.shields.io/npm/l/jest-dev-server.svg?style=flat-square
[license]: https://github.com/smooth-code/jest-puppeteer/blob/master/LICENSE
