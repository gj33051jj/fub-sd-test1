Things to note on the Fubarino Mini:

1. Pin 3 can not be used under sketch control if USB is being used (it is taken over by the USB hardware)

2. Pins 14 and 15 are connected to the 8MHz crystal. You can not use them under sketch control unless you disable the crystal and use the internal RC oscillator instead. (and possibly unsolder the crystal, depending upon what you need to do with the pins. Note that the crystal is needed to do anything with USB.)

