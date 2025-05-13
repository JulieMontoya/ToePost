# ToePost
A(nother) PET memory test utillity

ToePost is a memory test utility for the Commodore PET, which concentrates on three specific areas.  These are:
+ The first 256 bytes of display memory
+ Locations &0100-&01FF, which the 6502 uses for its hardware stack
+ Locations &0000-&00FF, which the 6502 treats as a memory-mapped register file extension

The intention being that if ToePost reports a pass, enough of the machine is in a working state
to be able to run more sophisticated programs.

## The Problem

The Commodore PET series of computers use the 6502 processor.  The design of the 6502 is such that it
expects at least the first 512 bytes of its addressable space to be populated with RAM.  The **JSR**
and **RTS** instructions store addresses on a stack, which is always implemented in addresses &0100
to &00FF.  
