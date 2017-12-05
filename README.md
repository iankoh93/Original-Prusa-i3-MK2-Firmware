# Original Prusa i3 MK2 Firmware

## General instructions

Pre-compiled hex output on PRUSA RESEARCH site: http://prusa3d.com/downloads/firmware/
Pre-compiled hex files for all printers by PRUSA RESEARCH are available in hex_files folder.
Just clone the repo and flash it to the firmware.


## Build instructions

### Step 1 - Download and Install/Unpack Arduino

Download and install Arduino; suggested and verified version is the 1.6.8 from:
https://www.arduino.cc/en/Main/OldSoftwareReleases#previous

If you also use Arduino for other purposes, it is suggested to use a stand-alone version rather than a system-wide install (e.g., use the "Windows" version instead of "Windows Installer"), and use it only for Prusa i3 MK2 firmware generation.

### Step 2 - Download PRUSA i3 MK2 Firmware

Clone the prusa3d/Prusa-Firmware (Branch: MK2) repository to a local path.

If you are not an expert git user, it is suggested to Download it as ZIP from:
https://github.com/prusa3d/Prusa-Firmware/tree/MK2

### Step 3 - Prepare the Arduino toolchain

#### Step 3a - Remove the LiquidCrystal library

Due to name collision, you have to remove the Liquid Crystal library of the original Arduino installation. To do so, you may rename the "Arduino\libraries\LiquidCrystal" directory to "Arduino\libraries\LiquidCrystal_", or remove it altogether.

#### Step 3b - Add RAMBo hardware / library support

Copy the contents of the ArduinoAddons/Arduino_1.6.x directory from the downloaded firmware repository to the root of the installed arduino path.

#### Step 3c - Add the bootloader

Download the bootloader image from:
https://raw.githubusercontent.com/arduino/Arduino/master/hardware/arduino/avr/bootloaders/stk500v2/stk500boot_v2_mega2560.hex

There is actually no difference between the hex file in the hardware / library support and the one linked above. Just copy over the stk500boot_v2_mega2560.hex from the stk500v2 folder into the bootloaders folder

Save a copy of it on each of the following directories:

    Arduino/hardware/arduino/avr/bootloaders/stk500v2/
    Arduino/hardware/marlin/avr/bootloaders/

Double-check that the file has the .hex extension and not .txt.

### Step 4 - Prepare the Arduino Firmware (For RAMBo13a boards)

####THIS ALREADY HAS BEEN DONE FOR THE FIRMWARE ON RAPID'S GITHUB

Copy the file Prusa-Firmware/Firmware/variants/1_75mm_MK2-RAMBo13a-E3Dv6full.h file (In the Variants folder)

Put it in the Firmware folder and rename it to : Configuration_prusa.h

(note that you may have to use the 1_75mm_MK2-RAMBo10a-E3Dv6full.h if you have an older RAMBo board.)

### Step 5 - Load and configure the Arduino IDE

Load the Arduino IDE and select the RAMBo as target board from the Tools submenu. You are now ready to modify and build the firmware!

### Step 6 [Optional] - Edit the Firmware

Make some modifications to the firmware.

For example, edit the Configuration_prusa.h file and edit (at your own risk):

    Extruder PID tuning: DEFAULT_Kp, DEFAULT_Ki and DEFAULT_Kd values
    Bed PID tuning: DEFAULT_bedKp, DEFAULT_bedKi, DEFAULT_bedKd values
    Extruder fan noise: reduce EXTRUDER_AUTO_FAN_SPEED from 255 to 96 (experiment a bit)

Additionally, Change the pre-heat settings as no one wants to preheat a heated bed.

### Step 7 - Build the Firmware

You can now build the firmware:

    Compile the firmware with Sketch -> Build (CTRL-R).
    Export the compiled binary with Sketch -> Export compiled sketch (CTRL-ALT-S).

You will find the exported binary in the Prusa-Firmware/Firmware directory:

    Firmware.ino.with_bootloader.rambo.hex
    
### Step 8 - Upload the Firmware

Upload the new firmare to the printer using the standard firmware update tool included in original Prusa firmware:
http://manual.prusa3d.com/Guide/Upgrading+firmware/66#_ga=1.48414886.1605935719.1473440900

