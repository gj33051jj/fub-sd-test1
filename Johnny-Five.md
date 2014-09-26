# John-Five compatiblity :-)
[Jounny-Five](https://github.com/rwaldron/johnny-five)

## Load firmata with chipKIT code
* Use MPIDE to load standardfirmata sketch to the Fubarino board

## create a new node.js project
* create the package.json file
```
{
  "name": "fubarino-io-examples",
  "version": "3.0.0",
  "description": "Provides a standard interface to boards capable of IO (e.g. chipKIT, Arduinos, Maestros, Raspberry Pis, etc) and their firmware",
  "main": "lib/ioboard.js",
  "repository": {
    "type": "git",
    "url": "git://github.com/ricklon/fubarino-io.git"
  },
  "dependencies": {
    "grunt": "~0.4.5",
    "grunt-contrib-jshint": "~0.10.0",
    "grunt-contrib-nodeunit": "~0.4.1",
    "grunt-contrib-uglify": "~0.5.0",
    "johnny-five":"latest",
    "fubarino-io":"latest"
  },
  "keywords": [
    "node",
    "firmata",
    "fubarino-io"
  ],
  "author": "Rick Anderson",
  "license": "BSD",
  "readmeFilename": "Readme.md"
}
```

* Do: ```npm install```

* create blink.js
```
var five = require("johnny-five"),
    board = new five.Board("fubarino-io");

board.on("ready", function() {
  // Create an Led on pin 13
  var led = new five.Led(21);

  // Strobe the pin on/off, defaults to 100ms phases
  led.strobe();
});
```
* run ```node blink.js```






