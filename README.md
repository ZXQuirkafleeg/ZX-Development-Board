# ZX128 CPLD Development Board

This is an altera Based development board for the RC2014 [(link)](https://rc2014.co.uk). The primary purpose is to provide a flexible hardware development environment, using a CPLD for glue and other logic, for the ZX128 Spectrum board [(link)](https://github.com/ZXQuirkafleeg/ZX128).

Notable features are:

* Direct Altera EPM7128SLC CPLD connection to the CPU bus for address decoding, I/O and signal processing
* Onboard ROM and RAM for prototyping memory mapped peripherals
* Support for various ROM and RAM (6264, 62256 and 628128) devices
* Prototyping area

![Front of ZX Development board with a prootype disk interface](https://github.com/ZXQuirkafleeg/ZX-Development-Board/blob/main/Images/ZX%20Dev%20V1.0%20-%20front.jpg)

## ROM

The board can take a variety of standard ROMs in U2.  The ROM jumpers are on the left edge of the board and the positions running from top to bottom for popular devices are:  

Jumper/Device	 | 28C64	| 28C256 | 27C64 | 27C256 | 27C512 | 39SF010 | 39SF020 | 39SF040
:---:          | :---: | :---:  | :---: | :---:  | :---:  | :---:   | :---:   | :---:							
A18	           | -     | -      | -     | -      | -      | -       | -       | -     
A17	           | 2-3	 | 2-3	  | 2-3   | 2-3    | 2-3	  | -       | -       | -
A16	           | -     | -      | -     | -      | -      | -       | -       | -     							
Pin 29 - Upper | 2-3	 | 2-3	  | -     | -      | -      | -       | -       | -
Pin 29 - Lower | -     | -      | 1-2   | 1-2    | 1-2    | 1-2     | 1-2     | 1-2
Pin 3 - Upper  | -     | -      | 2-3	  | 2-3	   | -      | -       | -       | - 
Pin 3 - Lower  | -     | 1-2    | -     | -      | 2-3    | 2-3     | 2-3     | 2-3

Other devices are supported.  The jumpers can be also used to ignore A14+ and select specific 8K ROM pages. 

A13-A18, /CS and /WE are directly connected to the CPLD and can be repurposed if no RAM is fitted.  /RD is connected to the CPU bus.

## RAM

The board can take 6264, 62256 or 628128 (or equivalent) RAMs in U3.  No jumper changes are needed.  A13-A16 and /CS are directly connected to the CPLD and can be repurposed if no RAM is fitted.  /WE and /RD are connected to the CPU bus.

## Prototyping area

The top half of the board provides a protyping area.  Left and right edge most vertical columns are 0V and 5V respectively.  Other pins are brought out to the separator row which divides the prototyping from the CPLD area.  Taking each group from left to right:

Pins          | Group     | Description
:---:         | :---:     | :---: 							
1             | 0V        |
2-11          | ROM/RAM   | CPLD H&A Outputs - pins 73-81 & 4-5 
12            | 0V        |
13            | 5V        |
14            | Clock     | CPU/CPLD Clock - pin 83
15-17         | CPLD I    | CPLD Global inputs - pins 1, 2 & 84
18-24         | CPLD I/O  | CPLD Block G I/O - pins 54-61
25-31         | CPLD I/O  | CPLD Block F I/O - pins 63-70
32-39         | D0-D7     | CPU Databus
40            | 5V        |

## RC2014 Bus

There are some differences between the RC2014 bus and the Spectrum edge connector signals.  Notably the CPU clock is inverted on the edge connector.  Spectrum peripherals also use ROMCS, NMI and WAIT (e.g. DIVMMC, Multiface and Inferface 1), so these are brought out to the user pins on the RC2014 bus:

*	Pin 37 – User 1 – ROMCS
*	Pin 38 – User 2 – NMI 
*	Pin 39 – User 3 – WAIT 
*	Pin 40 – User 4 – (unallocated)

## VHDL Code

A template Quartus 13.1 project can be found [here](https://github.com/ZXQuirkafleeg/ZX-Development-Board/tree/main/CPLD).  This project provide a basic framework for development and includes pin assignments.

## RC2014 Bus

There are some differences between the RC2014 bus and the Spectrum edge connector signals.  Notably the CPU clock is inverted on the edge connector.  Spectrum peripherals also use ROMCS and NMI (e.g. DIVMMC and Multiface), so these are brought out to the user pins on the RC2014 bus:

*	Pin 37 – User 1 – ROMCS
*	Pin 38 – User 2 – NMI 
*	Pin 39 – User 3 – (not connected, though later used for WAIT)
*	Pin 40 – User 4 – (unallocated)
 
## Build
All components are though-hole and standard density.  A schematic can be found [here](https://github.com/ZXQuirkafleeg/ZX-Development-Board/blob/main/PCB/ZX%20Dev%20-%20Schematic%20-%20V1.1.pdf) and a BOM [here](https://github.com/ZXQuirkafleeg/ZX-Development-Board/blob/main/PCB/BOM%20-%20Dev%20Board%20-%20V1.1.csv).

### PCB 

For the PCB, EasyEDA (V6.5.22) design files can be found [here](https://github.com/ZXQuirkafleeg/ZX-Development-Board/tree/main/PCB) and gerbers [here](https://github.com/ZXQuirkafleeg/ZX-Development-Board/blob/main/PCB/Gerber_PCB%20-%20RC2014-%20Dev%20V1.1%20-%20freeroute.zip).  The PCB is a standard two layer board approx. 100mm by 100mm.  Decoupling capacitors for the CPLD are mounted under the socket.

### CPLD Programming

Programming the CPLD can be done by the Programmer app in Quartus 13.1.  Connect a USB Blaster dongle (or clone) to J4 and connect the 5V supply to the ZX develoment board.  

### ROM Programming

Programming the ROM can be done by using a TL866II or similar.  There is no provision for programming the ROM in-situ though this should be possible for devices that can be programmed with a single 5V supply with appropriate CPLD code.

## Jumpers and Connectors

* J1 – RC2014 Bus connector
* J3 – ROMCS and NMI jumper – used connect ROMCS to U1 and NMI to U2 of RC2014 bus
* J4 – Altera USB Blaster ICSP connector

## System Test

Programme the ROM with a replacement Spectrum ROM (e.g. interface 2 game or Retroleum) in the lower 16K and install the appropriate ROM jumpers.  Jumper J3 so ROMCS is connected to the RC2014 bus.  Programme the CPLD with the test POF.

Install the CPU, ZX128 and Development boards in the backplane and apply power.  Current draw of the development board is around 150 mA making a total of 350 mA for the system
