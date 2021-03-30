# RPI-PICO-I2C-LCD
This is a project which adapts code from another user to allow usage of the PCF8574 I2C lcd backpack for either 20x4 or 16x2 lcd screens.

Credit: https://github.com/dhylands/python_lcd/tree/master/lcd mostly to Dave Hylands for the basic api and lcd driver code.

Project: Check it out for a full step-by-setp guide on Instructables: https://www.instructables.com/RPI-Pico-I2C-LCD-Control/ 

This is code adaptded for micropython and the Raspberry Pi PICO specifically.

Usage: 
- Download all 3 .py files included. 
- Open Thonny IDE with the 3 files
- Make pin edits or setup changes (See below for options) 
- DO NOT EDIT FILE NAMES!
- In Thonny, go to top menu File => Save Copy => Raspberry Pi Pico and save each file to the board with the same name as downloaded and with a .PY extension when saving it to the board. 
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

Functions / Usage: 

These are the python commands used in a program! (They can all be found in the lcd_api.py file with definitions to their functions)
- lcd.putstr("Text goes here!")                     - Send a string of chars to the display IMPORTANT: Use this for printing a variable: lcd.putstr(str(Variable)) [Turns variable into string] 
- lcd.show_cursor() / lcd.hide_cursor()             - Show / Hide the cursor of the lcd (White bar)
- lcd.blink_cursor_on() / lcd.blink_cursor_off()    - Turn on / Off the blinking cursor upon printing
- lcd.backlight_on() / lcd.backlight_off()          - Turn on / Off backlight of the LCD (Controlled by a small transistor on the backpack)
- lcd.display_on() / lcd.display_off()              - Turn on / Off the display (Not backlight but the entire chip)
- lcd.clear()                                       - Clear all chars or anything written to the display
- lcd.move_to(Col, Row)                             - Move to position based on row and col values (Y, X)
- lcd.custom_char(Num, bytearray([HEX chars])))     - Num can be any integer 0 - 8 (Writing to CGRAM locations) merely used for numbering. The HEX chars are simply made by using this link: https://maxpromer.github.io/LCD-Character-Creator/. It will provide a string of Hex charecters which can replace the "HEX chars" in the example command.

Errors:

OSERROR : 5 (This is quite a common error, 5 means I/O error. Check Your connections. This means codes can't be sent or recieved ensure SCL and SDA are properly connected through the level translator!

Feel to leave comments or questions / issues and I will try to answer / resolve them as quick as possible!

For people wondering how to use this code, Tinkernut on Youtube created a very nice guide + some more features! https://www.youtube.com/watch?v=B8Kr_3xHjqE&t


