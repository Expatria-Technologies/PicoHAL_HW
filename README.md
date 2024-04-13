![Logo](/readme_images/logo_sm.jpg)
# Flexi-HAL CNC Controller
<img src="/readme_images/Board_Photo.jpg" width="800">


Expatria Technologies GRBLHAL and LinuxCNC (and more!) CNC control board

Currently available in our online store:

https://expatria.myshopify.com/products/picohal

Please consider buying a board to support our open-source designs. 

The Flexi-HAL was designed to be an EMI resistant IO platform for any microcontroller based CNC/motion control firmware or software.  This board includes a few features that we couldn't find on other boards, and it reduces the amount of extra wiring in our setups.  In the co-operative spirit of the PrintNC and other CNC communities, and Open Source Hardware, the Flexi-HAL will be licensed and free to use by all parties, including commercial parties, under the CERN-OHL-S V2 license.  It is our hope that the community finds the design useful and that it may be carried forward to help advance the PrintNC and broader CNC hobby community.

The Flexi-HAL incorporates community driven elements from the PrintNC Electronic Standardization (EST) Project.  As part of this project, two additional breakout boards have been created for the user controls and limits/probe inputs.  These are simple boards and could easily be milled and hand assembled, but fabrication files for each are available in the CAM_Outputs folder.  The inputs are accessible via the RJ45 connectors on the Flexi-HAL mainboard.  In addition, the Flexi-HAL is intended to be used with the Expatria Real-Time jog controller or similar peripheral:

The key features of the PicoHAL:

  - Familiar Ardunion Uno form factor
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

LinuxCNC Remora/Ethernet component is located here:   
https://github.com/Expatria-Technologies/Remora-RP2040-W5500

## PicoHAL Overview

<img src="/readme_images/board_top.jpg" width="700">

Bottom side pin labels:

<img src="/readme_images/backside.png" width="700">

Pinout List:

<img src="/readme_images/Pinout.png" width="900">

### RP2040 Microcontroller

The PicoHAL is based around the Raspberry Pi RP2040 microcontroller.  The unique features of this MCU allow the PicoHAL flexibility to support both USB, Ethernet and RS485 connectivity.  The large onboard flash chip (128 Mb) allows the PicoHAL to provide significant local storage for Micropython firmware, gcode scripts and the GRBLHAL webui.

### UF2 Bootloader
The PicoHAL microcontroller has a UF2 bootloader built in. This allows you to upgrade or change the firmware on the flexi as easily as copying a file to a USB drive.  To enter UF2 mode, hold the BOOT button while pulsing the RUN (reset) button or while applying power/connecting the USB cable.

Once in bootloader mode, the Flexi will appear as a USB storage device called "RPI-RP2".  Simply copy the new firwmare to this USB drive and the board will automatically install it and reboot.

To use the PicoHAL with

### Power Input
The PicoHAL can be powered either externally with 5-24V or via USB.  The main Neopixel LED power pin is connected to the main input power.

### Shield Compatibility
<img src="/readme_images/Stepper_Pins.jpg" width="300">

The stepper drivers are designed to be used with IDC connectors that are quick to assemble.  Unfortunately you will need to ensure that at the external driver the high and low signal pairs are connected correctly as there is no standard pinout on these drivers.  The 8 pin connection allows you to run a high and low pair for every signal to ensure the best possible signal integrity.  The Flexi-HAL uses high speed digital isolators and differential RS-422 style signal drivers for the motion signals.

When using the alarm input the external drivers need to be configured for open-drain, active-low output.  The alarmmoutput must be high impedance during normal operation.

Typical wiring for most open-loop stepper drivers: 

![image](https://github.com/Expatria-Technologies/Flexi-HAL/assets/6061539/89e5df1e-06ef-4319-acb0-a30ee8e6447b)


### Analog Spindle Control

Traditional GRBL spindle control interface for 0-10V or 0-5V spindle control.  Uses a dual-stage output driver for linear response.  Spindle power supply is selectable between 5V, 12V or external.  Note that when running the board with less than 14v input, it may not be possible to reach the full 10V output level - this is due to the dropout of the 12V LDO.  In this case, use the external spindle power input to connect your 12v and bypass the LDO for the spindle control voltage.

<img src="/readme_images/Spindle_PWM_Config.jpg" width="500">

Near the main power input of the Flexi-HAL there is a diagram showing how a set of jumpers may be configured to enable 0-10V analog or TTL PWM output.  This jumper allows you to have a 12V compliant TTL PWM signal to drive a device like a laser engraver or an ESC.

Two headers - 3 pin P6 and 2 pin P16, connect the analog spindle section to the rest of the board.  P6 allows you to power the spindle section from either the onboard 5V or 12V rails.  If you remove P6 and P16 then the spindle section is completely isolated from the rest of the system and in this configuration can be used to drive motor controllers that require a floating control voltage.  Also, when driving the board with less than 14V input, it may not be possible to adjust the spindle output voltage to the full 10V.  In this case we recommend removing P6 (leave P16 in place) and applying 12V to the external spindle supply input directly.


### RS485 Spindle Control
This interface is primarily intended to be used with a Huanyang style VFD for spindle control.  The A, B and G (common) pins are marked on the top and bottoms sides of the PCB.  Simply connect the appropriate pins to the terminals on the VFD.  Note that the G pin should be used for signal common, it should not be connected to the shield of a shielded cable.

https://github.com/grblHAL/Plugins_spindle/

### 5 Axis limit inputs

By default both GRBL and the Flexi-HAL expect NPN NC limit switches.  PNP switches are not supported. NO switches can also be used on any switch input.

The first four axes have single limit inputs that are also accessible via the RJ45 EST limit breakout connector.  GRBL always knows the direction of travel so individual min and max pins are not required.  Auto-squaring is supported by enabling ganged axes in GRBLHAL and setting the appropriate pins.

In addition to the limit signals, there are two probe input pins on the limit RJ45 breakout connector and the main PCB that are multiplexed via XOR logic and share a single input pin on the microcontroller.

<img src="/readme_images/XOR_switches.gif" width="300">

For the dual-input signals there is no need to terminate unused ports.

The RJ45 pinout:

<img src="/readme_images/limit_rj45_pinout.jpg" width="150">

### User Buttons

Standard CNC functions are mapped to 4 inputs.  These signals are primarily intended to be used via the user RJ45 connector.  They are also exposed via 3 wire connections on the main PCB.  These inputs have the same circuitry as the limit inputs and are NPN.  Connect SIG and GND to assert the signal.  When multiplexed these signals must be NO logic.

The RJ45 pinout:

<img src="/readme_images/user_rj45_pinout.jpg" width="150">

The HALT signal is not a safety feature and should not be used in place of a true electrical emergerncy stop.  It is intended to notify the controller of urgent requests and should be NO as it is shared between the PCB terminal block, RJ45 output and motor alarm.

### HALT Polarity Selection

<img src="/readme_images/haltsel.png" width="400">

Starting from A5 revision, the polarity of the HALT signal to the MCU can be inverted by moving the jumper pictured above to the leftmost two pins.  This allows you to connect an NC overtravel sensor or NC e-stop circuit to the Flexi-HAL without the need for an external relay.  You must ensure that the HALT signal is not asserted (red light is not on) when in the nominal operating condition.

### Spindle, Flood and Mist relay drivers
The Spindle, Flood and Mist relay outputs are driven from the main board supply.  External relays should be selected to match the power supplied to the Flexi-HAL.  The absolute maximum coil current for each output should not exceed 250mA.  In general, the coil resistance should be 150 Ohm or greater.

### Auxillary relay drivers
<img src="/readme_images/AUX_POWER.jpg" width="200">
Four axilliary relay outputs are exposed.  These have a maximum combined drive current of 1000 mA when operated via a dedicated power supply.  The relay voltage can be selected via a 3 pin jumper between the main board power supply, the onboard 12V supply and the onboard 5V supply.  If the jumper is left unpopulated power for the aux outputs is supplied via the dedicated (fused and polarity protected) input.  The 12V and 5V options cannot drive more than 20 mA per pin and are only used for TTL signalling applications - they should never be used to drive inductive loads.  Never populate P17 and the external supply at the same time.  When driving external relays, the coil resistance should be 150 Ohm or greater.

### Real-Time Control Port
<img src="/readme_images/Jog2k_Enclosure_2.png" width="500">
This port is intended to allow for external pendant type devices to issue real-time jogging and override controls to the motion  controller.  It uses I2C signalling and adds additional signals for the keypad interrupt as well as the Halt signal.  We feel that a robust and wired control is the safest way to interact with a CNC machine in real time.  A simple reference controller implementation is under active development, but there are some code examples referenced in the GRBLHAL I2C keypad plugin repository:

https://github.com/grblHAL/Plugin_I2C_keypad/

https://github.com/Expatria-Technologies/RT_Jog_Controller/

### Spindle Sync Port
This port allows a differential connection to an external module for a robust GRBLHAL lathe implementation or to support a high-speed encoder input for LinuxCNC.  An encoder such as E6B2-CWZ1X is suitable for most spindle applications.

There is also a small breakout available that exposes these signals for 5V single-ended NPN input to interface with more basic sensors and other switched inputs.

The RJ45 pinout:

<img src="/readme_images/encoder_rj45_pinout.jpg" width="150">

### Raspberry PI expansion header
<img src="/readme_images/Pi_Installed.png" width="500">
The Rasberry Pi GPIO header allows the Flexi-HAL to host a full Raspberry Pi type SBC.  This allows the platform to support LinuxCNC via the Remora project, as well as hosting senders such as cnc.js or Gsender in Host mode.

<img src="/readme_images/Pi_Pinout.jpg" width="500">

### Accessories
Some 3D printed accessories are avilable in [Mods & Accessories](https://github.com/Expatria-Technologies/Mods-Accessories/), including a DIN rail mount and enclosures/mounts for the limit and button breakouts.

### Default Jumper Locations
![image](https://github.com/Expatria-Technologies/Flexi-HAL/assets/6061539/06c76aa8-7ccc-4621-a8f2-30bbd142a144)
By default the following jumpers (shown in the graphic in RED) should be populated.
This has the following effects:
The RPI Header uart is connected to the internal MCU UART - this is necessary to flash Remora firmware from the RPI and also is used with the uFlexiNET module for the SD card chip select.
The HALT polarity is set for use with an NO button like the button breakout.
The analog spindle output is set for 0-10V.
The analog spindle section is connected to the onboard 12V supply.
The analog spindle section ground is connected to the PCB external ground.
The aux outputs are powered via the main 24V input.


### Example Wiring Diagram
A comprehensive wiring diagram has been developed by the PrintNC community using the FlexiHAL.  While not specific to Flexi, this gives a great example of how to connect the board into the rest of a complete electronics box to drive a CNC machine. 
https://wiki.printnc.info/en/v3/wiring

### Attributions
This project uses components from the very helpful actiBMS library.

https://github.com/actiBMS/JLCSMT_LIB



