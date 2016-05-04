
## Summary

This project is an SOC (System on a Chip) coded in VHDL and implemented for the Lattice iCE40-hx8k dev board. The SOC contains the following components: 1802 CPU + UART + Timer + I/O Ports

## Required Hardware

* Lattice iCE40-hx8k dev board (can be ordered online at www.latticesemi.com)
* USB-to-Serial 3.3V adapter (can be ordered from eBay)
* misc USB cables and wires for connecting the USB-to-Serial adapter

NOTE: Make sure the USB-to-serial adapter is a 3.3V version. Some adapters have 5V interface signals which could damage your iCE40-hx8k dev board.

## Tools

* IceCube2 (from Lattice Semiconductor) was used for synthesis and FPGA Routing.
* Icestorm (https:/github.com/cliffordwolf/icestorm) was used for programming.


## Build Flow

I used the Lattice IceCube2 software to generate the SOC_bitmap.bin programming file and
then I used this command line "iceprog SOC_bitmap.bin" the program the iCE40-hx8k dev board
over the USB cable (iceprog is part of the icestorm tool suite).

## Console Interface

I used the minicom program (on Ubuntu Linux) as a console to communicate with the 1802 SOC over the USB-to-Serial connection. Configure minicom using the command line "minicom -s" to configure the serial port for ttyUSB0 and turn of the hardware handshaking. There are probably other alternatives to minicom. Any ANSI terminal-emulator program should work for this application.

## Pinout

The iCE40 pins are defined as follows:
```
UART_RXD   G1 -pullup yes
UART_TXD   G2
PORTA[0]   B5
PORTA[1]   B4
PORTA[2]   A2
PORTA[3]   A1
PORTA[4]   C5
PORTA[5]   C4
PORTA[6]   B3
PORTA[7]   C3
RESET      N3 -pullup yes
CLK        J3 -pullup yes
```

## 1802 CPU Background Info

The 1802 was a simple CMOS microprocessor introduced in 1974. It was an 8 bit processor, with 16-bit addressing and a large register file of 16 16-bit registers. It ran at a clock speed of 6.4 MHz at 10V (very fast for 1974).  Its static design allowed the clock to be stopped for power savings. All arithmetic and memory access instructions used the 8-bit accumulator D.  A typical program sequence might be: memory to D loads then D to registers opcodes, followed by D to memory stores, using one 16-bit register as an address.
A 4-bit control register P selected any one general register as the program counter, while control registers X and N selected registers for I/O Index and the operand for the current instruction. All instructions were 8 bits.  No subroutine support and no stack ops but clever use of registers allowed these to be implemented. For example, changing P to another register allowed jump to a subroutine. On an interrupt P and X were saved, then R1 and R2 were selected for P and X until an RTI restored them.

## Contributors

* Scott L Baker - SOC design
* Juergen Pintaske - 1802 evangelist and 1802 software contributor

## License

See the **LICENSE** file in this repository
