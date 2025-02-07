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


CYW43 Init No Delay
-------------------

Ths patch removes the 1000ms delay before intialization of the CYW43
driver. See <https://github.com/adafruit/circuitpython/pull/10038>.

Download: [patches/cyw43_init_nodelay.patch](patches/cyw43_init_nodelay.patch)


RP2xxx Low-Power
----------------

This patch reimplements light-sleep/deep-sleep from the alarm-module to
provide aggressive power-saving. It also implements the alarm-module for
the RP2350.

Due to the aggressive power-saving connections are lost during light-sleep
and on-going activities (e.g. playing music) will stop. It is the duty
of the application code to deinit/reinit any peripherals cleanly before
and after light-sleep.

**Note**: this patch already integrates the cyw43_init_nodelay.patch.

Download: [rp2xxx_low_power.patch](patches/rp2xxx_low_power.patch)
