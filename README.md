# RPI-PICO-I2C-LCD
This is a project which adapts code from another user to allow usage of the PCF8574 I2C lcd backpack for either 20x4 or 16x2 lcd screens.

Credit: https://github.com/dhylands/python_lcd/tree/master/lcd mostly to Dave Hylands for the basic api and lcd driver code.

This is code adaptded for micropython and the Raspberry Pi PICO specifically.

Usage: 
- Download all 3 .py files included. 
- Open Thonny IDE with the 3 files
- Make pin edits or setup changes (See below for options) 
- DO NOT EDIT FILE NAMES!
- In Thonny, go to top menu File => Save Copy => Raspberry Pi Pico and save each .py file to the board.
- Switch to the pico_i2c_lcd_test.py (this is the main file) and click run. This should be able to initalize the LCD display if settings are right.
- If you get errors, see below for a known list of errors and their fixes
- Wiring Diagram LCD_bb.jpg! Please look here for a fritzing diagram!

Requirements:
- 3.3 - 5V level translator. This is crucial to encure the lcd recieves the commands properly. I recommend this: https://www.adafruit.com/product/757 (Must be Bi-Directional)
- PCF8574 I2C LCD backpack. (These are common to find)

Setup Changes:
- Make sure the top address is set correctly!
Use this small program to scan for I2C devices:

import machine
sda=machine.Pin(0)
scl=machine.Pin(1)
i2c=machine.I2C(0,sda=sda, scl=scl, freq=400000)
print(i2c.scan())

- Once you get an address through the console (REPL), this will be in decimal and not hex. You can convert the decimal to hex or simply put a decimal address in the setup.
in my case, the decimal addr. was 39 which converts to 0x27 in hex.
- Ensure that your SCL and SDA pins are selected properly in accordance with the Pico's pin table. These connect to the low voltage side of the translator with a 3.3V Reference from the board. The high voltage side gets a 5V reference from the VBUS pin of the Pico.
- Finally, assure the I2C_NUM_ROWS and I2C_NUM_COLS are set properly!

Usage: 

Printing is simple :lcd.putstr("") This requires a string input! if you want to feed a changing value such as a temperature, it must be: lcd.putstr(str(Variable))

Errors:

OSERROR : 5 (This is quite a common error, 5 means I/O error. Check Your connections. This means codes can't be sent or recieved ensure SCL and SDA are properly connected through the level translator!

Feel to leave comments or questions / issues and I will try to answer / resolve them as quick as possible!
