# CircuitPython Driver for TEA5767 FM Radio Module

![41015747](https://user-images.githubusercontent.com/44191076/64875299-62e6e300-d67f-11e9-92d2-b0bdd43494aa.jpg)

This driver is the CircuitPython port of the [MicroPython version](https://github.com/alankrantas/micropython-TEA5767) with essentially the same interface. **See the MicroPython docs for instruction details**.

This driver has been tested on Adafruit ItsyBitsy M0, Adafruit Metro M4 and RPi Pico running CircuitPython 7.2.5

## Import and Initialize

Copy either ```TEA5767.py``` or ```TEA5767.mpy``` (recommended) into the ```lib``` directory of your device drive.

To import and initialize the module:

```python
import busio, board
from TEA5767 import Radio
    
i2c = busio.I2C(scl=board.SCL, sda=board.SDA, frequency=400000)
radio = Radio(i2c)
# or
radio = Radio(i2c, freq=106.7)

print(f'Frequency: FM {radio.frequency}')
print(f'Ready: {radio.is_ready}')
print(f'Stereo: {radio.is_stereo}')
print(f'DC level: {radio.signal_adc_level}')
```

> Change the pins since some boards may not have ```SCL``` and ```SDA```-named pins.

## A Simplified CircuitPython Version Without Using This Driver

This is the same one as in the MicroPython docs but use the CircuitPython I2C object instead:

```python
import busio, board

i2c = busio.I2C(scl=board.SCL, sda=board.SDA, frequency=400000)

def radio_frequency(freq):
    freqB = 4 * (freq * 1000000 + 225000) / 32768
    while not i2c.try_lock():
        pass
    i2c.writeto(0x60, bytearray([int(freqB) >> 8, int(freqB) & 0XFF, 0X90, 0X1E, 0X00]))
    i2c.unlock()
    
radio_frequency(106.7)
```
