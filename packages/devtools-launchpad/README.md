# Launchpad

[![Npm version](https://img.shields.io/npm/v/devtools-launchpad.svg)](https://npmjs.org/package/devtools-launchpad)

![launchpad-screenshot](https://cloud.githubusercontent.com/assets/1141550/22296671/433a4c68-e31b-11e6-8e46-bf3c653f6f6a.png)


### Starting

The launchpad is almost always run in conjunction with a devtool like the Debugger.
If you would like to run the launchpad by itself to test something or add a feature that is possible as well
by running `yarn start`.


The Launchpad makes it easy to build a developer tool for Firefox, Chrome, and Node.

[Debugger.html](../../README.md) and [Console.html](https://github.com/jasonlaster/console.html) are a good examples of tools built on top of the toolbox.


**Features**
* *Dev Server* - local development environment to run your tool
* *Webpack Base* - webpack config to build on top of
* *Landing Page* - see available connections
* *Bootstrap function* - hook to start your tool with a debuggee connection
* *Configs* - config system to add additional runtime configuration


#### Dev Server

* serve an `index.html` root
* serve JS bundles with incremental builds and hot-reloading
* handle cross origin requests from the client
* runs firefox's tcp-ws proxy

Example [dev-server.js](https://github.com/jasonLaster/console.html/blob/master/bin/dev-server.js)

```js
toolbox.startDevServer(envConfig, webpackConfig);
```

#### Webpack Config

The webpack [base config](./webpack.config.js) makes it easy to use the launchpad out of the box.

**Features**

* transpiles source: strips flow types, convert async to generators
* loads JSON files for L10N strings and Configs
* loads SVGs for inlining assets
* ignore modules that should be excluded (fs)
* CSS & JS hot reloading
* map shimmed modules to privileged modules when bundling for the panel
* bundles CSS into one file when building for the panel

Here's an example tool    [webpack.config.js](https://github.com/jasonLaster/console.html/blob/master/webpack.config.js).


#### Landing Page

The [Landing Page](./src/index.js)  shows the available Chrome, Firefox, and Node tabs to debug.

**Features**
* shows available connections
* has tools title
* sets up L10N
* loads the light, dark, and firebug themes

[Screenshot ](https://cloud.githubusercontent.com/assets/254562/20671763/a749acfa-b54c-11e6-9a4a-6b0fc4f45589.png)

#### Bootstrap function

The bootstrap function starts the launchpad and provides a connection hook for doing post-connect setup with the debuggee connection.

**Features**
* checks for a connection id i.e. `firefox-tab=child1/tab1`
* gets available tabs
* connects to a debuggee tab
* renders either the Landing Page or tool root component

```js
bootstrap(React, ReactDOM, App, actions, store)
  .then(conn => onConnect(conn, actions));
```


#### Configs

The launchpad has a [config system](../devtools-config/README.md) for adding runtime configs.

**Features**
* *target configs* - firefox, chrome, node configuration
* *feature flags* toggle features in the build
* *themes* - enable light, dark, firebug
* *hot-reloading* - toggle hot Reloading
* *logging* - enable different logging support
* *environments* - different configs for development, ci, panel
* *local override* - local config overrides

Here's an example [config](https://github.com/jasonLaster/console.html/blob/master/configs/development.json).
