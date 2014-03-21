##Connecting the Raspberry PI to the Fubarino serial ports

The RaspberryPi uses 3.3v logic and so does the ChipKit Fubarino SD board. The RaspberryPi conveniently has power, ground, tx, and rx pins right next to each on the GPIO pins.

###RaspberryPI GPIO Pinout
![RaspberryPi GPIO](http://www.adafruit.com/adablog/wp-content/uploads/2012/06/GPIOs.png) 

##Power Options
You can pick 3.3v, or 5v pins to connect to Vin on the Fubarino SD. Then match in convenient ground pins between boards. I recommend pin 6 GND on the RaspberryPi.

##Serial wiring options
For Serial the Fubarino SD you can use pins 8rx,9tx or pins 28rx, 29tx and connect them to pins 8tx,10rx on the RaspberryPi. 

Then load a simple Hello World sketch that prints "Hello World 0|1" to Serial0, or Serial1 depending on which port is wired to RaspberrPi GPIO pins 8,10.

##Configure the RaspberryPI

The wiring is now complete, but the default configuration of the RaspberryPI serial port is be used as an alternative system console. You will need to disable all of that. 

1. Edit /etc/inittab
        `sudo vi /etc/inittab'
        Replace:
        `T0:23:respawn:/sbin/getty -L ttyAMA0 115200 vt100`
        with:
        `#T0:23:respawn:/sbin/getty -L ttyAMA0 115200 vt100`
2. Remove console error debugging from 
`sudo vi /boot/cmdline.txt`
Remove:
`console=ttyAMA0,115200 kgdboc=ttyAMA0,115200`
From:
`dwc_otg.lpm_enable=0 console=ttyAMA0,115200 kgdboc=ttyAMA0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait`
Leaving:
`dwc_otg.lpm_enable=0 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait`
3. Reboot the Raspberry PI


##Now test from Raspberry PI
1. Reboot
2. screen /dev/ttyAMA0

If you don't have screen:
`sudo apt-get install screen`

Have Fun!


The above information was derived from the following information is what I did following the directions at Hobby Tronics [RaspberryPi Serial Port Configuration](http://www.hobbytronics.co.uk/raspberry-pi-serial-port).




 