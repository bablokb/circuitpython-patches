Low-Power Patch for RP2xxx
==========================

This patch

  - implements the `alarm`-module for RP2350
  - vastly improves the existing `alarm`-module for RP2040
  - returns the correct pin in gpio-triggered wake-alarms  
    (object `alarm.wake_alarm`)
  - `microcontroller.cpu.reset_reason` now correctly returns
    `DEEP_SLEEP_ALARM` instead of `SOFTWARE`
  - implements `rtc.calibration` for the RP2350 low-power oscillator


Alarm-Module for RP2350
-----------------------

The `alarm`-module is still missing in mainline CircuitPython. This
patch adds the complete functionality including sleep-memory.

The priority is on low-power. For light-sleep, the implementation uses
the "dormant"-mode of the RP2350, for deep-sleep it used
"PowMan"-domains.  PowMan supports up to four GPIOs for wake up. For
details about these modes, see the section below.

Due to the aggressive power-saving connections are lost during light-sleep
and on-going activities (e.g. playing music) will stop. It is the duty
of the application code to deinit/reinit any peripherals cleanly before
and after light-sleep.

This is no real limitation since it is not possible to keep
peripherals running *and* save a relevant amount of power. If you need
to keep things running, use `time.sleep()` and/or spinning on GPIOs
instead.

The only reason not to use this patch is if you used light-sleep in your
code while keeping peripherals active and (in the case of the Pico-W)
expect WLAN-connections to persist.

In this case you won't loose anything if you replace light-sleep
with normal sleep/spinning.


Alarm-Module for RP2040
-----------------------

The implementation now also uses "dormant"-mode for pin-alarms. In addition,
it shuts down the CYW43 of the Pico-W during light-sleep. This reduces
current draw dramatically:

           |   LS-TA |   LS-PA | DS-TA  | DS-PA  |
-----------|---------|---------|--------|--------|
Pico       |  1.20mA |  0.78mA | 1.20mA | 0.78mA |
  (10.0.3) | 13.7 mA | 14.0 mA | 6.85mA | 0.95mA |
Pico-W     |  0.43mA |  0.34mA | 0.44mA | 0.34mA |
  (10.0.3) | 35.9 mA | 35.9 mA | 1.45mA | 0.38mA |


Correct Pin in `alarm.wake_alarm`
---------------------------------

Mainline does not provide the pin that triggered the wakeup from deep-sleep.
With this patch, the field `alarm.wake_alarm.pn` is set correctly.


Correct `microcontroller.cpu.reset_reason`
------------------------------------------

After wake up from deep-sleep, the RP2040 implementation resets the cpu
(like `microcontroller.reset()` does). After such a reset the field
`microcontroller.cpu.reset_reason` is set to `ResetReason.SOFTWARE`
instead of `ResetReason.DEEP_SLEEP_ALARM`. This also trips the logic
within `main.c`.

The patch fixes this for the RP2040 and implements it for the RP2350.
