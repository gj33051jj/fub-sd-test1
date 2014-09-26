# John-Five compatiblity :-)
[Jounny-Five](https://github.com/rwaldron/johnny-five)

## Load firmata with chipKIT code
* Use MPIDE to load standardfirmata sketch to the Fubarino board

## create a new node.js project
* create the package.json file
* Update the dependencies to include:
** fubarino-io
** johnny-five

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

## TODO: Create IO Plugin for Fubarino Boards
* Clone https://github.com/achingbrain/node-ioboard
* Modify into fubarino-io and chipKIT-io




