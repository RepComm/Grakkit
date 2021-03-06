## Grakkit

It's the fusion of GraalVM and Bukkit.

## Installation

You will need to run your server with GraalVM. Download one of the archives listed, extract the contents somewhere, and use `bin/java` as your java path when launching a server.

**GraalVM Download:** [Windows](https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-20.0.0/graalvm-ce-java11-windows-amd64-20.0.0.zip) | [Mac](https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-20.0.0/graalvm-ce-java11-darwin-amd64-20.0.0.tar.gz) | [Linux](https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-20.0.0/graalvm-ce-java11-linux-amd64-20.0.0.tar.gz)

## Command: JS

The `js` command is used to evaluate code in-game. It tab-completes like a DevTools console.

## Command: Module

The `module` command is the secret sauce of grakkit.

To add a module to your server, use `/module add <repo>`. To remove a module, use `/module remove <repo>`. And, to update a module, use `/module update <repo>`.

Once you've added a module to your server, use `require(<repo>)` to execute the code within.

In any case, `<repo>` can refer to one of the following:

-   A speed-dial keyword from the [official module list](https://github.com/hb432/grakkit/blob/master/modules.json).
-   A GitHub repository, in `username/repository` format.

## Modules

To create your own module, you can use [this repository](https://github.com/hb432/grakkit-test/) as a starting point.

You will need a GitHub repository with a `package.json`, which must contain a `main` field pointing to a JS file within the repository.

The `module` command makes use of your latest release for both `add` and `update` functionality, so you will need to publish your repository at least one time to use your module in a public environment.

Module scripts will be execute in the global scope, so, to avoid pollution, you may wrap your code in an anonymous function. From within the global scope, set the value of `globalThis.exports` to return that value when the script is required.

## Example

This module will return a random number from 1 to 10. Notice how auxillary files can be required via their relative path from the current file.

**./package.json**

```json
{
    "name": "rand",
    "main": "./index.js"
}
```

**./index.js**

```js
const rand = require('./lib/rand.js')
module.exports = rand.range(1, 10)
```

**./lib/rand.js**

```js
module.exports = {
    int: function (limit) {
        return Math.floor(Math.random() * Math.abs(limit + 1))
    },
    range: function (min, max) {
        return jx.rand.int(max - min) + min
    },
    chance: function (chance) {
        return Math.random() < chance
    }
}
```
