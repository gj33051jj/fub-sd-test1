# How Do I
====
* Enter programming mode

Before you can compile and upload a sketch to the FubarinoSD, you must put it into programming mode. This is very simple - simply press and hold the PRG button while you press and release the RESET button. Then release the PRG button. If programming mode was successfully entered, the green LED will rapidly blink. You can then click the Upload buttion (right pointing arrow) in MPIDE and if your code compiles properly, it will be uploaded.

Note that the serial port that MPIDE uses to upload new code to the FubarinoSD board does not exist until you put the board into programming mode. So it will not show up in the Tools->Serial Port menu until you put it into programming mode.

* Find the stk500v2 driver for Windows

When you download MPIDE, the driver for the USB serial port (which Windows will ask you to find when you first plug the Fubarino SD in to your computer) is located at \mpide-0023-windows-20120903\drivers\Stk500v2.inf
