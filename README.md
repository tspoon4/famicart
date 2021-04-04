# Overview
A while ago, I bought a Famicom in Japan with a bunch of game carts (including Super Mario and Zelda!)
As a programmer, I always had in mind to run my own program on it and play with this amazing hardware.
As a beginner in electronics, designing a development cartridge would be an good opportunity to learn.

After a bit of googling, I found the Nesdev community and their extensive [knowledge base](http://wiki.nesdev.com/w/index.php/Nesdev_Wiki). For this learning project, I wanted to design the board from scratch by choosing myself the components and re-discovering the boolean logic to connect them.

I enjoyed studing the various IC datasheets and playing with the Arduino to prototype the PRG-ROM Flasher.
Please note that this project is not optimized for cost and is not a professional development platform.

# Features
* 512KiB PRG-ROM (Flash) split in 16x32KiB banks
* 64KiB PRG-RAM (SRAM) split in 8x8KiB banks, battery backed work RAM
* 16KiB CHR-RAM (SRAM) with 2-4 nametable support
* Simple discrete logic mapper using an 8bit write-only register at address $8000
	* Bit 0-3: PRG-ROM 32KiB bank selection (2^4 = 16)
	* Bit 4-6: PRG-RAM 8KiB bank selection (2^3 = 8)
	* Bit 7: Software mirorring control (horizontal, vertical)
* CHR-RAM memory mapping customization with 2 jumpers
	* Jumper J2: CIRAM enable/disable
	* Jumper J3: Software controlled mirroring/single-screen
* Arduino Uno/Nano PRG-ROM programmer is available at this [location](flash/)
* KiCad source files for modification are available at this [location](kicad/)

# Pictures
Images | Images
------ | ------
![](images/famicart_brb.jpg?raw=true) | ![](images/famicart_pcb.jpg?raw=true)
![](images/famicart_sld.jpg?raw=true) | ![](images/famicart_fnl.jpg?raw=true)

# Production
To produce the PCB that will fit in the Famicom cartridge slot, you will have to provide to the manufacturer the 2 following items:
* The PCB thickness should be **1.2mm**
* The Gerber files for the different PCB layers from this [location](gerber/)

# Components
Reference | Quantity
--------- | --------
Resistor 1k | 1
Resistor 10k | 1
Diode 1N4148 | 2
Capacitor 100nF | 7
Nand 74HC00 | 2
Nor 74HC02 | 1
Latch 74HC573 | 1
Flash SST39SF040 | 1
SRAM uPD431000A | 2
Conn 3pins | 2
CR2032 Holder | 1

# Potential improvements
### Palette RAM bus conflict when disabling CIRAM
When disabling CIRAM the CHR-RAM is always enabled because:
* CHR-RAM /CE1 is connected to GND
* CHR-RAM CE2 is connected to VCC

CHR-RAM read/write technically mirrors Palette RAM indices but this has not been extensively tested.
### Component packing
Packing is loose and PCB surface use can be improved to reduce costs and fit in a cartrige package.
Vias close to the cartridge connectors might be saved by leveraging memory chip pads.
### CHR-RAM banks instead of PRG-RAM
I've made the choice to implement banking on PRG-RAM instead of CHR-RAM.
It is possible to modify the KiCad source files to use Bit 4-6 of the mapper register to implement banking on CHR-RAM the same way as PRG-RAM.

# References
* [Nesdev wiki](https://wiki.nesdev.com/w/index.php/Cartridge_connector) for the cartridge connector pinout
* [Nesdev forums](https://forums.nesdev.com/viewtopic.php?f=9&t=2162) for the PCB specs and various information
* [KiCad EDA](https://kicad.org/) to edit the PCB source files
* [Arduino Nano](https://store.arduino.cc/usa/arduino-nano) for the links on datasheets
* [NES ca65 example](https://github.com/bbbradsmith/NES-ca65-example) to validate the cartridge features 
* IC Datasheets (TI, SST, NEC)

