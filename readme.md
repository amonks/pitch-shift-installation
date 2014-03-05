# client-side pitch-shift installation

The problem with pitch-shift is the examples. The super-bare-bones minimal example they provide doesn't actually play any audio, so it's hard to tell if it works. 

The online demo requires 2 packages that aren't required by pitch-shift itself, so if you try to run its `index.html` locally without installing those first, it throws hella errors.

Here I'll take you through both install processes: how to install the non-functional super-bare-bones minimal pitch-shift example, and how to install the working demo. Here goes.

## browserify

First you'll need browserify, node normally installs packages into the working directory (so that each project / library / whathaveyou can have its own version of each dependency without causing conflicts, which is awesome). Use the `-g` flag to install browserify globally, since it's a command-line tool rather than a project dependency.

	sudo npm install browserify -g

## minimal pitch-shift install

### files (`minimal/`):

`./app.js` (from the Pitch-shift readme):

	//Create a pitch shifting object
	var shifter = require("pitch-shift")(
	  function onData(frame) {
	    //Play / write out frame.  Called whenver data is ready
	  },
	  function onTune(t, pitch) {

	    console.log("Got pitch ", pitch, " at time ", t)

	    //This is the amount to scale the sample by
	    return 1.0
	  })


	//Feed some data to the shifter
	shifter(new Float32Array([1, 1, 0, 1, 0, 0, 0 /* ... etc */ ]))

`./index.html`:

	<!DOCTYPE html>
	<html>
	<head>
		<title>pitch-shift demo</title>
	</head>
	<body>
		<script src="bundle.js"></script>
	</body>
	</html>



### install pitch-shift itself:

	cd ./minimal
	npm install pitch-shift
	browserify ./app.js -o ./bundle.js

### Done!

Pitch-shift is now installed into `./minimal`. If you open up `index.html` you'll notice that it doesn't actually do anything.

## functional demo install

That wasn't very useful, let's try to run the [demo](http://mikolalysenko.github.io/pitch-shift/).

### files (`demo/`)

[`./index.js`](https://github.com/mikolalysenko/pitch-shift/blob/master/example/index.js)

[`./index.html, ./gettysburg.mp3, ./guitar_c.mp3`](https://github.com/mikolalysenko/pitch-shift/blob/master/example/www)

### install pitch-shift + app dependencies

our `./index.js` file two dependencies of its own that are *not* required by pitch-shift itself. We'll need to install these manually.

	cd ./demo
	npm install pitch-shift
	npm install typedarray-pool
	npm install domready

### Done!

You're done! Note: if you try to open up `file:///.../demo/index.html` in your browser, you'll get a cross-origin request error because your computer isn't serving the mp3s properly. 

You'll have to move `demo/` do a webserver of some kind to get it to run. I'm on OSX, so I like to use OSX's built-in apache server. If you've never used it, you can turn it on by running `sudo apachectl start`, and then it'll automatically serve anything in `/Library/WebServer/Documents` at `http://localhost`.

