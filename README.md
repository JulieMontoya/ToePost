# ToePost
A(nother) PET memory test utillity

ToePost is a memory test utility for the Commodore PET, which
concentrates on three specific areas.  These are:
+ The first 256 bytes of display memory
+ Locations &0100-&01FF, which the 6502 uses for its hardware stack
+ Locations &0000-&00FF, which the 6502 treats as a memory-mapped register file extension

The intention being that if ToePost reports a pass, enough of the
machine is in a working state to be able to run more sophisticated
programs.

## The Problem

The Commodore PET series of computers use the 6502 processor.  The
design of the 6502 is such that it expects at least the first 512 bytes
of its addressable space to be populated with RAM.  The **JSR** and
**RTS** instructions store addresses on a stack, which is always
implemented in addresses &0100 to &01FF.  (The stack pointer is only
8 bits wide; when the processor is addressing the stack, the high bits
of the address bus are always %00000001.)  There are also some special
addressing modes which work on addresses in the range &0000-&00FF,
either individually  (you can think of them as extra 8-bit registers)
or in pairs to hold addresses for indirect addressing.

Therefore, **it is not safe use any instruction which relies on the
stack or zero page until the integrity of the necessary memory has
been proved.**  This means no **JSR**, **RTS**, **PHA**, **PLA**,
**PHP** or **PLP**; and no **(short, X)** or **(short), Y**
addressing modes.  Furthermore, **until we have proved at least some
memory to be usable, we can only hold values in registers.**  This
means we can only use the accumulator, the X and Y registers and the
stack pointer.

## Memory Faults

There are three types of fault that will affect RAM.  These are bits
that always read as 0, irrespective what be written to them, and to
which I might refer as **stuck zeros**; bits that always read as 1, or
**stuck ones**; and locations that return values written to other
locations, or **address clashes**.

We can test for stuck bits by writing a known value to a location and
trying to read it back; and we can test for address clashes by reading
other locations to see whether they still contain the expected values.



## How ToePost tests memory

ToePost first zeroes out a block of memory at &8000 to &80FF.  It then
works through each location in turn, writing the value %00000001 there.
This location is read back and **EOR**ed with the accumulator contents,
which will yield something other than %00000000 if that location is
faulty.  Such a condition would be reported as a write error.

If the display memory is functioning, a write error will be displayed
below the area under test, looking something like

**a7f w01 r00**

This means that at **address** &80**7f**, we **w**rote &**01** and
**r**ead back &**00**.

The machine will then go into a "catch groove" -- an infinite loop,
requiring a power cycle.  If the display memory is really, really badly
faulty, you might only be able to tell what is happening by the steady
values on all address lines but A0 and A1.

If the readback is successful, ToePost searches downwards from the
_immediately preceding_ location towards &8000, looking for non-zero
values which would indicate a fault -- either a bit stuck high, or an
address clash within the lower order bits causing values to be read
back from locations other than where they were written to.  Such a
condition would be reported as a read error.

Upon reaching &8000, ToePost searches upwards from the _next_ location
_after_ the one just written, towards &80FF, looking for non-zero
values which would be reported as a read error.

If the display memory is functioning, a read error will be displayed
below the area under test, looking something like

**a07     r10**

This means that at **address** &80**07**, where we were expecting &00,
we **r**ead &**10**.

The machine will then go into a "catch groove".

If we reach &80FF without reading anything but a zero at any location
(except the one just written, which we skipped both times because we
know it will not be zero)  then the relevant bit of the location just
written has passed; and we repeat the process, writing %00000010 this
time, reading back and looking for non-zero values everywhere else, all
the way up to %10000000.

After all 8 bits in a memory location have been tested and passed,
ToePost moves on to the next location; and so on until every bit within
every byte has been successfully written and read back, without
disturbing the contents of any other location in the block under test.

