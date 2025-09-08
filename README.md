# SmartPendant

Project of SmartPendant for grblHAL

![Image](https://github.com/Devtronic-US/SmartPendant/raw/main/Media/Devtronic_SmartPendant_Case.png "Devtronic SmartPendant Case")

## Software

To clone the project:

```
git clone --recurse-submodules <url>
```

### Building using the IDE
    
You can import the project into [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html).

You can then download the firmware with [STM32CubeProg](https://www.st.com/en/development-tools/stm32cubeprog.html).

### Building from the command line

Prerequisites:

* gcc C/C++ compiler (arm-none-eabi-gcc)
* CMake (version 4.0.0 or newer)

Run CMake:

```
mkdir build
cd build
cmake -DCMAKE_TOOLCHAIN_FILE=./cmake/arm_none_eabi_gcc.cmake -DCMAKE_BUILD_TYPE=Debug ..
```
Now you can build the project using `make`.

## Hardware

Fully assembled custom board is available here (US only): https://devtronic.square.site/

To make this project yourself, you will need three essential parts:

* [WeAct BlackPill F411 25M HSE:](https://s.click.aliexpress.com/e/_DC6TlGd)

* [3.5" Display with touchscreen based on ILI9488 LCD controller and FT6236 touch controller](https://www.aliexpress.us/item/3256804935586911.html)

* [60 mm 6 pin 100 PPR handwheel](https://s.click.aliexpress.com/e/_DCFuJHr)

This handwheel also works with 3.3V. The STM321F411 pins that the handwheel is connected to are 5V tolerant, so it can be powered from 5V.

## Schematic

See https://github.com/bullestock/SmartPendantHardware.

# Precompiled Firmware

Latest firmware HEX file can be found in Release folder: [SmartPendant.hex](https://github.com/bullestock/SmartPendantFirmware/blob/main/Release/SmartPendant.hex)

To load new firmware [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html) is used:
1) If your device programmed with version **0.027.0** or later, hold top edge (MPG) button. Otherwise (not programmed, update failure, older version) hold BOOT0 button on the back instead.
2) Connect SmartPendant to PC using USB-C cable (it should be powered only from the USB!) or hit reset button on the back if it already connected and powered.
3) Couple seconds later release MPG or BOOT0 button.
4) Open STM32CubeProgrammer. In top right corner choose "USB" from drop down list.
5) If field "Port" in "USB Configuration" show "No DFU detected" click update button near it.
6) Click "Connect" button - STM32CubeProgrammer should establish connection and show current device memory content.
7) Click "Open File" in left to corner, select firmware HEX file, then click "Download" button in top left corner.
8) When flashing is done, close STM32CubeProgrammer and power cycle the device or short press NRST button on the device to restart it. 

**Note:** If during update you get an error, you may try to use a different cable, another PC, connect device to the PC via USB hub (that usually helps for some reason) or slightly heatup STM32F411 MCU with hair dryer.
This error appears because STM32 bootloader uses internal RC oscillator to measure crystal osillator frequency. According to Application Note AN2606 external crystal oscillator can be in range 4...26 MHz with 1 MHz step. However higher frequencies have lower error margin which in some cases can cause incorrect frequency detection and errors during bootload process.

Starting from version **0.027.0** if firmware detects that top edge (MPG or KEY on BlackPill) button is pressed, it calibrates internal RC oscillator and makes jump to bootloader, which should mitigate problem described above.

# Thanks

Many thanks to Terje who made this project possible by adding another datastream to grblHAL.

Thanks to Nicolai Shlapunov who initially created this project.
