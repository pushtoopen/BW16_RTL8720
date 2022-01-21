# BW16_RTL8720
Instructions on how to interface with the B&amp;T/BW16-RTL8720 rtlduino demo board

Here is the toolchain: https://github.com/ambiot/ambd_sdk
this toolchain uses mbed OS on top of FREERTOS
Windows toolchain (with 32 bit cygwin only????) seems to be the only one that allows for flashing.
https://www.cygwin.com/setup-x86.exe
During install for cygwin, select "make" and "bc" packages
I've already tried their linux build process with both a segger JTAG and just a USB->UART and it does not work immediately

inside the cygwin. git clone git@github.com:ambiot/ambd_sdk.git

navigate to $clone_loc/ambd_sdk/project/realtek_amebaD_va0_example/GCC-RELEASE/project_lp/
execute: make all

navigate to $clone_loc/ambd_sdk/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp/
execute: make all (this is a REALLY LONG build)



to edit code, mod files here: 
ambd_sdk/project/realtek_amebaD_va0_example/src/src_lp/main.c
ambd_sdk/project/realtek_amebaD_va0_example/src/src_hp/main.c
(copy the realtek_amebaD_va0_example folder to make your own project)
driver examples here: https://github.com/ambiot/ambd_sdk/tree/master/project/realtek_amebaD_va0_example/example_sources

Use this tool to flash:
ambd_sdk/tools/AmebaD/Image_Tool/ImageTool.exe
after make all is done, select browse on the top row and grab this file: $clone_loc/ambd_sdk/project/realtek_amebaD_va0_example/GCC-RELEASE/project_lp/asdk/image/km0_boot_all.bin
next select browse on the 3rd  row and grab this file: $clone_loc/ambd_sdk/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp/asdk/image/km4_boot_all.bin
next select browse on the 4th  row and grab this file: $clone_loc/ambd_sdk/project/realtek_amebaD_va0_example/GCC-RELEASE/project_hp/asdk/image/km0_km4_image2.bin

put the board in download mode by holding the reset button then holding the burn button, then letting go of the reset button then letting go of the burn button in that order.

select the correct COM port for your usb->UART cable and hit the download button to flash 
hit reset on the board to get the chip out of download mode
use putty or some other serial monitor and set the baud to 115200 and the COM port of the FTDI cable to see whats going on.

To connect to RTL8720/B&T:BW16 "rtlduino" board, connect the following:
FTDI cable: 
RX (yellow) connects to pin "GA4" or "LOG_TX" on the board 
TX (orange) connects to pin "GE1" or "LOG_RX" on the board
GND (black) connects to any GND pin
5V (red) connects to "VIN" pin

Do not try to use 3.3V FTDI cable. I attempted it and it looks like the firmware is browning out.

connect to serial using BAUD 115200 (this IS be the port to flash against)

if plugged into the USB on the board, serial must be BAUD 38400
This is an interface to command AT only 
When sending AT commands, send each instruction with \r\n on the end (this is usually a setting in serial software)
once plugged in and listening to either serial port at the correct baud rate, press reset button on board to see output


"AT commands" are a format of https://en.wikipedia.org/wiki/Hayes_command_set
AT Commands listed here:
https://github.com/ambiot/ambd_sdk/blob/master/doc/AN0025%20Realtek%20at%20command.pdf


from the "RTLDUINO" board maker
https://www.aliexpress.com/item/1005001768102716.html?spm=a2g0s.9042311.0.0.4efe4c4drYBZ26
There are two serial ports in module BW16. One is AT serial port (the default baud rate is 38400), which is used for receiving and receiving AT instructions. The corresponding port numbers are AT_TX and AT_RX.One is the LOG serial port (baud rate 115200), used to print logs and download firmware, for port numbers LOG_TX,LOG_RX.

 Distribution network

BW16 AT serial port default baud rate is 38400, send each instruction with \r\n end

After getting the module, the first thing is to connect 5V,GND,LOG_TX,LOG_RX, and send instructions with LOG serial port ATSC=0 to switch to OTA1 image, otherwise, part of the module AT serial port sends instructions without response.


