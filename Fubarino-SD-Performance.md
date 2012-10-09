The Fubarino SD is a very fast board. The processor runs at 80MHz and because it is 32bits can do quite a bit of work on each cycle. This page documents some performance numbers that you can use when deciding if Fubarino SD is the right tool for your project.

## SD Card Read/Write

With the simple SD card library included with MPIDE as of 10/09/2012, the Fubarino SD can write to micro SD at about 100KB/s, and read at about 100KB/s. This depends somewhat on the exact card used.

## GPIO pin toggle

Because the MIPS core on the Fubarino SD runs at 80MHz, you can generate a 40MHz square wave (i.e. one pin toggle every 25ns) as the fastest GPIO operation. Note that in order to achieve this speed, you must bypass the Arduino pin abstraction and write directly to the PIC32 registers.