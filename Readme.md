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

Download: [ili9488_waveshare.patch](9.2.x/ili9488_waveshare.patch)


RP2xxx Slim
-----------

This patch tweaks a number of default configuration settings to
increase free RAM (with 9.2.6: about 14K more free RAM at startup).

Download: [rp2xxx_slim.patch](9.2.x/rp2xxx_slim.patch)


RP2xxx Fix-Connect
------------------

The RP2xxx wifi boards usually need more than one connect attempt. The
reason is that the code does not honor the timeout-parameter and bails
out after the first check of the link-status after starting to connect
asynchronously.

This fix automatically rechecks the link-status during the timeout
period, either until the timeout expires or until a successful connect
is detected.

Download: [rp2xxx_fixconnect.patch](9.2.x/rp2xxx_fixconnect.patch)


RP2xxx Low-Power
----------------

This patch reimplements light-sleep/deep-sleep from the alarm-module to
provide aggressive power-saving. It also implements the alarm-module for
the RP2350.

Due to the aggressive power-saving connections are lost during light-sleep
and on-going activities (e.g. playing music) will stop. It is the duty
of the application code to deinit/reinit any peripherals cleanly before
and after light-sleep.

Download: [rp2xxx_low_power.patch](9.2.x/rp2xxx_low_power.patch)


ESP32AT
-------

This patch enables <https://github.com/bablokb/circuitpython-esp32at> as
a submodule and adds it as a frozen module to various boards.

Download: [esp32at.patch](9.2.x/esp32at.patch)


ACEP No Clean
-------------

This patch disables the extra clean cycle during refreshing of ACEP
e-paper displays. This time-consuming operation is not necessary with
every refresh and can be triggered manually from application side by
sending a pure white image to the display.

Downlaod: [acep_no_clean.patch](9.2.x/acep_no_clean.patch)
