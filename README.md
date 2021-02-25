# BW16_RTL8720
Instructions on how to interface with the B&amp;T/BW16-RTL8720 rtlduino demo board

ubuntu:
sudo apt-get update
sudo apt-get install libc6-i386
sudo apt-get install libncurses5-dev
sudo apt-get install make (check this by running make before you try to install it, likely unnecessary)
sudo apt-get install bc (may not be necessary)
sudo apt-get install gawk


make a dev folder and navigate into it.
git clone git@github.com:ambiot/ambz2_sdk.git
cd /ambz2_sdk/project/realtek_amebaz2_v0_example/GCC-RELEASE
make all
navigate to /ambz2_sdk/tools/amebaZ2/image_tool/
unzip the linux tool and navigate inside the tool
run AmebaZII_PGTool_Linux_VX.X.X_UI
use this tool to flash

under /ambz2_sdk/doc/AN0500 Realtek Ameba-ZII application Note.en.pdf 
page 39 tells you how to use this application. the "flash_is.bin" file
is under  /ambz2_sdk/project/realtek_amebaz2_v0_example/GCC-RELEASE/application_is/Debug/bin/flash_is.bin

To connect to RTL8720/B&T:BW16 "rtlduino" board, connect the following:
FTDI cable: 
RX (yellow) connects to pin "GA4" or "LOG_TX" on the board 
TX (orange) connects to pin "GE1" or "LOG_RX" on the board
GND (black) connects to any GND pin
5V (red) connects to "VIN" pin

Do not try to use 3.3V FTDI cable. I attempted it and it looks like the firmware is browning out.

connect to serial using BAUD 115200 (this SHOULD be the port to flash against)

if plugged into the USB on the board, serial must be BAUD 38400
This is an interface to command AT only 
When sending AT commands, send each instruction with \r\n on the end (this is usually a setting in serial software)

once plugged in and listening to either serial port at the correct baud rate, press reset button on board to see output


"AT commands" are a format of https://en.wikipedia.org/wiki/Hayes_command_set

every processor is different, so that will only give you a limited understanding of the commands.

Finding the list of "AT" commands is becoming difficult. This may be unnecessary if you flash this processor with 
https://github.com/ambiot/amazon-freertos

This lists that amebaz2 is supported for this and the below link describes the rtl8720 as an ameba z2.

https://www.cnx-software.com/2019/07/16/ameba-z2-iot-board-realtek-rtl8720cm-armv8-m-km4-secure-wireless-mcu/ 

only issue is that this module is a B&T BW16, not necessarily an "amebaz2"

So i'm still trying to figure all this out. Please feel free to let me know if you have any more info on this.

This is a different board with this chip on it:
https://www.mikroe.com/wifi-11-click

It seems to have a bunch more funcitonality and support. I have yet to dig through it.

This comes up when i google "BW16 AT Commands" but i additionally havent dug through it yet:
https://files.seeedstudio.com/wiki/Air602_WiFi_Module/res/Air602%20AT%20Command%20User%20Manual.pdf


from the "RTLDUINO" board maker
https://www.aliexpress.com/item/1005001768102716.html?spm=a2g0s.9042311.0.0.4efe4c4drYBZ26
There are two serial ports in module BW16. One is AT serial port (the default baud rate is 38400), which is used for receiving and receiving AT instructions. The corresponding port numbers are AT_TX and AT_RX.One is the LOG serial port (baud rate 115200), used to print logs and download firmware, for port numbers LOG_TX,LOG_RX.

 Distribution network

BW16 AT serial port default baud rate is 38400, send each instruction with \r\n end

After getting the module, the first thing is to connect 5V,GND,LOG_TX,LOG_RX, and send instructions with LOG serial port ATSC=0 to switch to OTA1 image, otherwise, part of the module AT serial port sends instructions without response.


