# YALR - Yet Another Live Reload

Free and open source headless daemon for [LiveReload][LiveReload]. Nothing more.
Works with every build tool out there.


## Installation

    [sudo] npm install -g yalr

## Usage

    yalr [options]

The `yalr` command will start a [LiveReload Protocol v7][protocol] compatible
Web Socket server listening on a port 35729 while watching all the
files for modifications in the current working directory and its
subdirectories. File/directory deletions and additions are considered as
modifications too. Just add this script tag to your app:

```html
<script>document.write('<script src="http://'
+ (location.host || 'localhost').split(':')[0]
+ ':35729/livereload.js"></'
+ 'script>')</script>
```

or use the LiveReload Chrome, Firefox or Safari [extension][extension] and you're good to go!

Now your web pages will be automatically reloaded every time when a change is
detected in the watch path.

The watch path and various other options can be passed to the `yalr` command via
command line switches (use `--help` to list them).

## yalr.js

Another method to configure YALR is via an `yalr.js` file which is read from
the current working directory or from a path specified with `--configFile`.

The `yalr.js` is a node.js module exposing a single object:

```javascript
module.exports = {

  // Watch files only in the public directory
  path: "public",

  // Reload page only when html, css or js file changes
  match: [
    "*.html",
    "*.css",
    "*.js"
  ]

};
```

You can use both methods simultaneously. Command line switches will override
the options defined in the `yalr.js` file.

## node.js API

YALR can be really easily be embedded in node.js applications.

Simplest use case:

```javascript
require("yalr")();
```

This is effectly same as executing the `yalr` command without options.

It can have options too:

```javascript
require("yalr")({
  path: "public"
});
```

It will also read the `yalr.js`. Options in the `yalr.js` will take precedence
over the options given using the API. Which makes it perfect for developer
specific config. Just put it to .gitignore.

The yalr module function returns an object with the current options and a `tag`
attribute which contains a string of the script tag. This can be used to make
more tight integrations to various node.js web frameworks. The example
directory contains an [example][express-example] using the Express framework.

## Options

Options for the `yalr.js` file, node.js API and the command line.

### port

Port to listen to.

*Default: 35729*

### path

Path to watch.

From the command line this can be defined multiple times and and from the `yalr.js`
it can be also an array.

*Default: (The current working directory)*

### match

Match only certain files.

Glob string or JavaScript RegExp object. The glob string will be matched against the
the basename and the RegExp object will be matched against the full file path.
Regexp format is not avaible from the command line.

From the command line this can be defined multiple times and and from the `yalr.js`
it can be also an array.

*Default: (anything)*

### ignore

Like match, but for ignoring files.

*Default: (nothing)*

### debounce

Debounce time in milliseconds before actually sending the reload.

In some cases, such as static site generators, you might have multiple
change events coming in for long periods of time. This option makes YALR to
wait until the events stop arriving before sending the reload request.

Default: 0

### sleepAfter

Sleep milliseconds after a reload.

In some cases a reload can cause other files to be generated which can lead in
the worst case to an infinite reload loop. Setting this to `1000` will prevent
that effectively.

*Default: 0*

### verbose

Print more debugging stuff.

*Default: false*

### quiet

Be totally silent.

*Default: false*

### configFile

Custom path for the `yalr.js` file.

Not avaible in the `yalr.js` :)

*Default: (The current working directory)*

### disable

Start YALR as disabled.

Useful for temporally disabling the embedded YALR server from the `yalr.js` file.

*Default: false*

### disableLiveCSS

Disable smart live CSS reloads.

*Default: false*

### beforeUpdate

Execute CLI command or Javascript function with callback before update.

Very useful for running SASS or CoffeeScript compilers.

## Thanks

Thanks to original [LiveReload][] project! If you're on OS X consider purchasing it since
It will also help this project too because this uses the MIT licenced [livereload.js][] 
component of LiveReload 2.


## License

The MIT License

[protocol]: http://feedback.livereload.com/knowledgebase/articles/86174-livereload-protocol
[extension]: http://feedback.livereload.com/knowledgebase/articles/86242-how-do-i-install-and-use-the-browser-extensions-
[LiveReload]: http://livereload.com/
[express-example]: https://github.com/epeli/yalr/tree/master/example/express-integration
[livereload.js]: https://github.com/livereload/livereload-js
