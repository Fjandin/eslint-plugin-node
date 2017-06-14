# Disallow `require()`s for files that don't exist (no-missing-require)

Maybe we cannot find typo of import paths until run it, so this rule checks import paths.

```js
// If the file "foo" doesn't exist, this is a runtime error.
const foo = require("./foo");
```

## Rule Details

This rule checks the file paths of `require()`s, then reports the path of files which don't exist.

Examples of :-1: **incorrect** code for this rule:

```js
/*eslint node/no-missing-require: "error" */

var typoFile = require("./typo-file");   /*error "./typo-file" is not found.*/
var typoModule = require("typo-module"); /*error "typo-module" is not found.*/
```

Examples of :+1: **correct** code for this rule:

```js
/*eslint node/no-missing-require: "error" */

var existingFile = require("./existing-file");
var existingModule = require("existing-module");

// This rule cannot check for dynamic imports.
var foo = require(FOO_NAME);
```

## Options

```json
{
    "rules": {
        "node/no-missing-require": ["error", {
            "allowModules": [],
            "tryExtensions": [".js", ".json", ".node"],
            "resolvePaths": ["/an/absolute/path"]
        }]
    }
}
```

### allowModules

Some platforms have additional embedded modules.
For example, Electron has `electron` module.

We can specify additional embedded modules with this option.
This option is an array of strings as module names.

```json
{
    "rules": {
        "node/no-missing-require": ["error", {
            "allowModules": ["electron"]
        }]
    }
}
```

### tryExtensions

When an import path does not exist, this rule checks whether or not any of `path.js`, `path.json`, and `path.node` exists.
`tryExtensions` option is the extension list this rule uses at the time.

Default is `[".js", ".json", ".node"]`.

### resolvePaths

Adds additional paths to try for when resolving a require.
The paths must be absolute.

Default is `[]`

## Shared Settings

The following options can be set by [shared settings](http://eslint.org/docs/user-guide/configuring.html#adding-shared-settings).
Several rules have the same option, but we can set this option at once.

- `allowModules`
- `tryExtensions`
- `resolvePaths`

```js
// .eslintrc.js
module.exports = {
    "settings": {
        "node": {
            "allowModules": ["electron"],
            "tryExtensions": [".js", ".json", ".node"],
            "resolvePaths": [__dirname]
        }
    },
    "rules": {
        "node/no-missing-require": "error"
    }
}
```
