# XFII Browser

## Overview

This repository holds the build tools needed to build the XFII desktop browser for macOS, Windows, and Linux. In particular, it fetches and syncs code from the projects like xfii-core and chromium

- [Chromium](https://chromium.googlesource.com/chromium/src.git)
  - Fetches code via `depot_tools`.
  - sets the branch for Chromium (ex: 65.0.3325.181).
- [xfii-core](https://github.com/SoftwarePrince/xfii-core)
  - Mounted at `src/brave`.
  - Maintains patches for 3rd party Chromium code.
- [adblock-rust](https://github.com/brave/adblock-rust)
  - Implements XFII's ad-block engine.
  - Linked through [brave/adblock-rust-ffi](https://github.com/brave/adblock-rust-ffi)

## Downloads

You can [visit our website](https://www.xfiitech.com/) to get the latest stable release.

## Contributing

Please see the [contributing guidelines](./CONTRIBUTING.md)

## Install prerequisites

Follow the instructions for your platform:

- [Windows](https://github.com/brave/brave-browser/wiki/Windows-Development-Environment)

## Clone and initialize the repo

Once you have the prerequisites installed, you can get the code and initialize the build environment.

```
git clone https://github.com/SoftwarePrince/xfii-browser.git
cd xfii-browser
npm install

# all these steps take 12-24 hous to run
# the Chromium source is downloaded which has a large history
npm run init
```

You can also set the target_os and target_arch for init and build using

## Build XFII

The default build type is component.

```
# start the component build compile
npm run build
```

To do a release build:

```
# start the release compile
npm run build Release
```

### Build Configurations

Running a release build with `npm run build Release` can be very slow and use a lot of RAM especially on Linux with the Gold LLVM plugin.

To run a statically linked build (takes longer to build, but starts faster)

```bash
npm run build -- Static
```

To run a debug build (Component build with is_debug=true)

```bash
npm run build -- Debug
```

You may also want to try [[using sccache|sccache-for-faster-builds]].

## Run XFII

To start the build:

`npm start [Release|Component|Static|Debug]`

# Update XFII

`npm run sync -- [--force] [--init] [--create] [xfii_core_ref]`

**This will attempt to stash your local changes in xfii-core, but it's safer to commit local changes before running this**

`npm run sync` will (depending on the below flags):

1. 📥 Update sub-projects (chromium, xfii-core) to latest commit of a git ref (e.g. tag or branch)
2. 🤕 Apply patches
3. 🔄 Update gclient DEPS dependencies
4. ⏩ Run hooks (e.g. to perform `npm install` on child projects)

### Scenarios

#### Create a new branch

```bash
xfii-core> git checkout -b branch_name
```

or

```bash
xfii-browser> npm run sync -- --create branch_name
```

### Checkout an existing branch or tag

```bash
xfii-core> git fetch origin
xfii-core> git checkout [-b] branch_name
xfii-core> npm run sync
...Updating 2 patches...
...Updating child dependencies...
...Running hooks...
```

or

```bash
xfii-browser> npm run sync --create branch_name
...Updating 2 patches...
...Updating child dependencies...
...Running hooks...
```

### Update the current branch to latest remote

```bash
xfii-core> git pull
xfii-core> npm run sync
...Updating 2 patches...
...Updating child dependencies...
...Running hooks...
```

#### Reset to latest xfii-browser master, xfii-core master and chromium

```bash
xfii-browser> git checkout master
xfii-browser> git pull
xfii-browser> npm run sync -- --init
```

#### When you know that DEPS didn't change, but .patch files did (quickest)

```bash
xfii-core> git checkout featureB
xfii-core> git pull
xfii-browser> npm run apply_patches
...Applying 2 patches...
```

# Enabling third-party APIs:

1. **Google Safe Browsing**: Get an API key with SafeBrowsing API enabled from https://console.developers.google.com/. Update the `GOOGLE_API_KEY` environment variable with your key as per https://www.chromium.org/developers/how-tos/api-keys to enable Google SafeBrowsing.


this browser was made by [Software Prince Company](https://SoftwarePrince.com)
