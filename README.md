![Logo](/readme_images/logo_sm.jpg)
# PicoHAL CNC Controller
<img src="/readme_images/Board_Photo.jpg" width="800">


Expatria Technologies GRBLHAL and LinuxCNC (and more!) CNC accessory board

The key features of the PicoHAL:

  - RP2040 MCU
  - W5500 Ethernet module
  - Familiar Arduino Uno inspired form factor
  - RS485 input and pass-through
  - 5-24V power input (can be USB powered)
  - Works with CNC shield
  - Works with Relay shield
  - Works with stackable relay shields

MicroPython firmware for the PicoHAL plugin is located here:  
https://github.com/Expatria-Technologies/picohal-firmware

For reference, the GRBLHAL PicoHAL plugin code is located here:   
https://github.com/Expatria-Technologies/Plugins_picoHAL

A basic GRBLHAL mapping for the CNC shield is located here:  
https://github.com/Expatria-Technologies/RP2040/tree/picohal_map

LinuxCNC Remora/Ethernet component is located here:   
https://github.com/Expatria-Technologies/Remora-RP2040-W5500

## PicoHAL Overview

<img src="/readme_images/board_top.jpg" width="700">



### RP2040 Microcontroller

The PicoHAL is based around the Raspberry Pi RP2040 microcontroller.  The unique features of this MCU allow the PicoHAL flexibility to support both USB, Ethernet and RS485 connectivity.  The large onboard flash chip (16 MB) allows the PicoHAL to provide significant local storage for Micropython firmware, gcode scripts and potentially the GRBLHAL webui.

### UF2 Bootloader
The PicoHAL microcontroller has a UF2 bootloader built in. This allows you to upgrade or change the firmware on the flexi as easily as copying a file to a USB drive.  To enter UF2 mode, hold the BOOT button while pulsing the RUN (reset) button or while applying power/connecting the USB cable.

Once in bootloader mode, the PicoHAL will appear as a USB storage device called "RPI-RP2".  Simply copy the new firwmare to this USB drive and the board will automatically install it and reboot.  Micropython firmware is loaded by following the RPI foundation documentation.

### Power Input
The PicoHAL can be powered either externally with 5-24V or via USB.  The main Neopixel LED power pin is connected to the main input power.

### Shield Compatibility
The PicoHAL should be compatible with most shields that work with the Uno form factor.  The only caveat is that the PicoHAL uses 3.3V logic levels on the shield connector.  This is to preserve high-speed capability on the inputs and outputs.  The GPIO numbers are listed on the board.  Pins that are optimized to be inputs (they have a low-pass filter) have a white highlight.  The low-pass filter can be bypassed by soldering the bridges on the back side of the board.  The output pins are not filtered.

Top side pin labels:

<img src="/readme_images/topside.png" width="700">

Bottom side pin labels:

<img src="/readme_images/backside.png" width="700">

### PWM Spindle Control
There is a 5V PWM output near the RJ45 connector that can be used for external PWM control for a laser or a second Neopixel string.


### RS485 Interface
The PicoHAL has an RS485 interface with two connectors for easy pass-through/daisy chaining.  The direction control pin is assigned to GPIO27.  If required, a 120R termination resistor can be connected by soldering the bridge on the bottom side of the board.


### Attributions
This project uses components from the very helpful actiBMS library.

https://github.com/actiBMS/JLCSMT_LIB



