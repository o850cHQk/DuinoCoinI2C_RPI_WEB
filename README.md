# DuinoCoinI2C_RPI_WEB
JK-Rolling's DuinoCoinI2C miner now with a webui basically the exact same as his repo except the AVR_Miner_RPI.py has been modified to put the worker information into a web front end.

## Video Tutorial

Raspberry Pi AVR I2C Bus 1 [tutorial video](https://youtu.be/bZ2XwPpYtiw)

Raspberry Pi AVR I2C Bus 0 [tutorial video](https://youtu.be/ywO7j4yqIlg)

Raspberry Pi Pico HW Part1 [tutorial video](https://youtu.be/yu4R4ktdubY)

Raspberry Pi Pico SW Part2 (coming soon)

## Python Environment Setup

### Linux

```BASH
sudo apt update
sudo apt install python3 python3-pip git i2c-tools python3-smbus screen -y # Install dependencies
git clone https://github.com/JK-Rolling/DuinoCoinI2C_RPI.git # Clone DuinoCoinI2C_RPI repository
cd DuinoCoinI2C_RPI
python3 -m pip install -r requirements.txt # Install pip dependencies
````

Use `sudo raspi-config` to enable I2C. Refer detailed steps at [raspberry-pi-i2c](https://pimylifeup.com/raspberry-pi-i2c/)

For RPI I2C Bus 0, extra step is needed, `sudo nano /boot/config.txt` and add `dtparam=i2c_vc=on`, save and reboot

For RPi Pico as worker, the RPi master is strongly recomended to use 400kHz/1MHz SCL clock frequency. again, `sudo nano /boot/config.txt`,
 add `dtparam=i2c_baudrate=1000000` and `dtparam=i2c_vc_baudrate=1000000` for 1MHz. Change to 400000 if you prefer 400kHz SCL.

Finally, connect your I2C AVR miner and launch the software (e.g. `python3 ./AVR_Miner_RPI.py`)

### Windows

1. Download and install [Python 3](https://www.python.org/downloads/) (add Python and Pip to Windows PATH)
2. Download [the DuinoCoinI2C_RPI](https://github.com/JK-Rolling/DuinoCoinI2C_RPI/releases)
3. Extract the zip archive you've downloaded and open the folder in command prompt
4. In command prompt type `py -m pip install -r requirements.txt` to install required pip dependencies

Finally, connect your I2C AVR miner and launch the software (e.g. `python3 ./AVR_Miner_RPI.py` or `py AVR_Miner_RPI.py` in the command prompt)

## Version

DuinoCoinI2C_RPI Version 3.3

# Arduino - Slave

Arduino shall use `DuinoCoin_RPI_Tiny_Slave` sketch. LLC is required if Arduino is operating at 5V and master at 3.3V. Arduino also have builtin temperature sensor that can be used for IoT reporting. This feature is enabled for fun as the ADC is uncalibrated. Once calibrated, the temperature can be pretty accurate according to some. Set `TEMPERATURE_OFFSET` and `FILTER_LP` to calibrate.

Occasionally slaves might hang and not responding to master. quick workaround is to press the reset button on the slave to bring it back.

Once in a blue moon, one of the slave might pull down the whole bus with it. power cycling the rig is the fastest way to bring it back.

To solve these issues permanently, update Nano with Optiboot bootloader. WDT will auto reset the board if there is no activity within 8s.

Change this line to `true` to activate the WDT. works for Nano clone as well. **make sure the Nano is using Optiboot**