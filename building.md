# Building ToePost

ToePost is written in [BeebASM](https://github.com/stardot/beebasm/tree/master).
The author acknowledges that this is a bit perverse for a PET program,
since BeebASM is surely somewhat specific to the BBC Micro; but "BBC
flavour" 6502 assembly language  (**&** introduces a hex constant,
a dot indicates a label definition, comments are preceded with a
backslash, BBC BASIC expressions are valid)  is the syntax I learned nearly four
decades ago, and I know I would have spent far too long cursing the
"un-BBC-like" syntax of any other assembler.

## INSTALL BEEBASM

You should only need to do this once, obviously .....

```
$ git clone https://github.com/stardot/beebasm.git
$ cd beebasm/src
$ make code
$ cd ..
$ sudo install -c beebasm /usr/bin/
```

## CONFIGURE THE SOURCE CODE

As supplied, the CRTC is initialised with parameters for a 50Hz (PAL)
machine.  A separate initialisation table is also provided for a 60Hz
(NTSC) PET.  Search the Source Code for `LDA crtc_data_pal` and follow
the instructions in the comments.

For PAL / 50Hz:
```
    LDA crtc_data_pal, X    \  Get register value
\     LDA crtc_data_ntsc, X   \  Get register value
```
For NTSC / 60Hz:
```
\     LDA crtc_data_pal, X    \  Get register value
    LDA crtc_data_ntsc, X   \  Get register value
```

If you need to use your own, different set of CRTC parameters for your
PET, just edit them in place of what's already there.  And please let
the author know, so your parameters can be included in a future version
of ToePost.  (This is GitHub, after all .....)

## BUILD THE ROM IMAGE

Type
```
$ beebasm -i ToePost.6502
```
to build the Source Code.  When this is done, there should be a file
called `toepost.bin` of size 2048 bytes.

## BURN THE IMAGE TO A ROM

The ROM image file you have just built should be programmed into a
blank 2716 EPROM and inserted in place of the EDITOR ROM in the PET.

It would also be possible to have a larger EPROM in some sort of
adaptor board, with jumpers or DIPswitches to force the higher order
address lines and so select a 2KB image.  This is beyond the scope of
these instructions, though.
