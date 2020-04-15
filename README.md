# snes2wii-teensy
An adaptation of snes2wii to the Teensy 2.0 (ATmega32U4).

All thanks goes to Raphnet who does 99.99% of the legwork for creating this code. I'm just some dude who wanted to run the code on a Teensy2 (since I'm too cheap to buy the adapter).

## the original snes2wii

<https://github.com/raphnet/snes2wii>

or

<https://www.raphnet.net/electronique/x2wii/index_en.php>

## setup

Follow this link <https://www.pjrc.com/teensy/first_use.html>

Generally:

1. `sudo apt-get install libusb-dev`
2. On ubuntu, we can install the teensy loader application by `sudo apt-get install teensy_loader_cli`
3. <https://www.pjrc.com/teensy/49-teensy.rules>,  `sudo cp 49-teensy.rules /etc/udev/rules.d/`
4. `sudo apt-get install gcc-avr binutils-avr avr-libc`


## make and install

Plug in the Teensy, then just `cd` into directory, `make`, `teensy_loader_cli --mcu=TEENSY2 -w snes2wii.hex`, and press the reset button.

## teensy2 pinout

WARNING! This is not the same pinout as the original code!

```
Jumpers
PB0 = Common
PB1 = JP0
PB2 = JP1

SNES
PB4 = Clock OUT
PB5 = Latch OUT
PB6 = Data IN

GameCube
PD0 = Data IN/OUT
PD2 = Debug (Unconnected)
```

I didn't use any additional circuitry. So you just connect the dots directly.

## pinout of controllers

<http://www.int03.co.uk/crema/hardware/gamecube/gc-control.htm>

<http://www.repairfaq.org/REPAIR/F_SNES.html>

## notes

Since the Teensy2 (ATmega32U4) generally shares common registers that the ATmega8 and ATmega168 that Raphnet uses for the snes2wii, I thought it would be simple to adapt the code to run on the Teensy2.

It turns out it was quite simple to adapt the code. First of all, the Teensy2 uses a 16MHz crystal... which is a major requirement for proper timing with the GameCube.

To get it to work on the Teensy2, I did:

1. Map all of Port C pins to Port B (since the Teensy2 doesn't route out the needed pins in the original code)
2. Remap Pin D2 to Pin D0 since INT0 exists at a different pin on the ATmega32U4
3. Make sure the Teensy2 runs at 16MHz (just needed to add some init code)

So not that much work 😀. All in all, I was quite surprised by how simple it was to adapt the code. This was due to how similar the AVR family is and generally means this code can easily be ported to other AVR chips (such as newer and cheaper ones).
