# Repro steps for crashing the TypeScript language service in VS Code

_This was tested on an Apple M1 Mac OS: 12.1 laptop but I've seen this issue on older Mac OS's and on the Intel processors. I was also running Node `v12.16.1`_

Install this VS code version:

```plaintext
Version: 1.63.2
Commit: 899d46d82c4c95423fb7e10e68eba52050e30ba3
Date: 2021-12-15T09:38:17.605Z
Electron: 13.5.2
Chromium: 91.0.4472.164
Node.js: 14.16.0
V8: 9.1.269.39-electron.0
OS: Darwin arm64 21.2.0
```

1. Install a package that reveals this issue such as: `npm i -D cypress@9.3.1`

1. Run `npm ci` to ensure you are only reproducing the issue related to the packages installed

1. Comment out all "Permutations" in the `tsconfig.json` file except for `Permutation H`

1. Open the `src/test.ts` file and keep that tab/window open

1. Reload VS Code via `Cmd+Shift+P` then choose `> Developer: Reload Window`  to initialize loading the TypeScript language -- the TypeScript language will crash (as expected)

Note that this issue isn't just specific to the `cypress` package but is related to something else as I've seen this issue in another project I work on that does not have `cypress` installed (I just happened to find another package that reveals this bug).

For example, if you uninstall the `cypress` package and instead install `lodash` via `npm i -D lodash@4.17.21` then `Permutation H` will not cause the TypeScript Language to crash in VS Code.

Therefore the true bug may be a combination of both `Permutation H` as well as `X` npm package (and `Y` sub-packages that are installed) in order to further narrow down why VS Code's TypeScript language plugin crashes.

## Extra Info

This issue doesn't reproduce on earlier versions of VS Code prior to the December 15th 2021 update.

For example, this issue doesn't reproduce with:

```plaintext
Version: 1.58.0
Commit: 2d23c42a936db1c7b3b06f918cde29561cc47cd6
Date: 2021-07-08T06:54:17.694Z
Electron: 12.0.13
Chrome: 89.0.4389.128
Node.js: 14.16.0
V8: 8.9.255.25-electron.0
OS: Darwin x64 17.7.0
```
