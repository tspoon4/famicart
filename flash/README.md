# Overview
This folder contains the Famicart SST Flash programmer for the Arduino.

The Arduino program waits for incoming data from the Serial interfact.
It first tries to decode a command and if a command is recognized, it will execute it immediately.
Input or output data is always transferred from the host PC through the Serial interface.

The 8bits of data are direcly applied to the SST chip from the Arduino pins.
The 24bits of address are transferred to the daisy-chained shift registers using the vanilla SPI library.

The program accepts 3 commands through Serial input:
* **erase**: erases the whole SST chip
* **write \[address\]**: writes at \[address\] the stream of bytes that will follow 
* **read \[address\] \[size\]**: reads at \[address\] an amount of \[size\] bytes and output to tty

# How to use (Linux terminal)
Assuming the Arduino is bound to ttyUSB0, first set the device properties

> stty -F /dev/ttyUSB0 speed 115200
> stty -F /dev/ttyUSB0 raw -echo -echoe -echok

Then in order to write the SST chip, start by erasing it and then send the command to write the data contained in the program.rom binary file (generally assembled with ca65)

> echo "erase" > /dev/ttyUSB0 ; sleep 1s
> echo "write 0" > /dev/ttyUSB0 ; cat program.rom > /dev/ttyUSB0

**Note that in order to succeed, another terminal has to consume the Serial output as follows**

> cat /dev/ttyUSB0

# Pictures
Image | Image | Image
----- | ----- | -----
![](../images/flash_ardy.jpg?raw=true) | ![](images/flash_front.jpg?raw=true) | ![](../images/flash_back.jpg?raw=true)

# Components
Reference | Quantity | Purpose
--------- | -------- | -------
Capacitor 100nF | 4 | Decoupling
Capacitor 10uF | 2 | Lower frequency decoupling
Arduino Nano v3.x | 1 | Interface between Host/Flash
Shift 74HC595 | 3 | Arduino pin expansion for address bus (SPI)
Breadboard MB-102 | 1 | A big generic IC socket
Patience | Some | Complete the circuit successfully

