
# Node.js

Installation instructions for the Tessel CLI can be found here: [http://tessel.github.io/t2-start/](http://tessel.github.io/t2-start/)

This document will address common problems related to Node.js usage.

## NVM

The official installation instructions assume Node is installed globally. For those of you using NVM this will not be the case. As indicated [here](https://github.com/creationix/nvm/issues/786) NVM does not install globally by design.

### Linux

Install `t2-cli` module as normal. To install the drivers, perform the following:

**Find where the `node` executable is located:**

```bash
$ which node
/home/username/.nvm/versions/node/v4.5.0/bin/node
```

In the example above, the path is `/home/username/.nvm/versions/node/v4.5.0/bin` (referred to as `$nodePath` below).

**Install the drivers.**

```bash
$ sudo PATH="$PATH:$nodePath" $nodePath/t2 install-drivers
```
