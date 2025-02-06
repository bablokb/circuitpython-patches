CircuitPython Patches
=====================

This is my collection of CircuitPython source-code patches. These patches
are either not suitable for upstream, weren't accepted or are just too
specific for personal needs.

All patches are provided "as is", they might or might not work (anymore). Only
apply these patches if you know why and what you are doing.


Waveshare ILI9488 Displays
--------------------------

Some of the Waveshare displays have a strange 16-bit addressing mode. This
patch fixes `shared-module/displayio/bus_core.c` to allow for this
addressing mode.

Download: [ili9488_waveshare.patch](patches/ili9488_waveshare.patch)


