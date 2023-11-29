# SAMEPiCar
Rasperry Pi controller SCX10 II Deadbolt, built as a project for the Saddleback College SAME chapter.

## Equipment Needed
* Soldering iron and solder
* Screen, mouse, and keyboard for Raspberry Pi

## Parts List
### Purchased
* [SCX10 II Deadbolt](https://www.axialadventure.com/product/1-10-scx10-ii-deadbolt-4x4-brushed-rtr-blue/AXI03025T1.html) ($340)- RC Car
* [3S Smart LiPo](https://www.axialadventure.com/product/11.1v-5000mah-3s-30c-smart-g2-lipo-battery-ic5/SPMX53S30.html) ($70) - Power
* [Smart Charger](https://www.axialadventure.com/product/s1100-g2-1x100w-ac-smart-charger/SPMXC2080.html) ($100) - Charger for battery
* [Raspberry Pi 4B](https://www.pishop.us/product/raspberry-pi-4-model-b-4gb/) ($55) - Computer for RC car control
* [32GB Micro SD Card](https://www.amazon.com/SanDisk-Extreme-microSDHC-UHS-3-SDSQXAF-032G-GN6MA/dp/B06XWMQ81P/ref=sr_1_6?crid=2P5JDFI5V5W8V&keywords=32gb+micro+sd+card&qid=1693351205&sprefix=32gb+micro+sd+car%2Caps%2C170&sr=8-6) ($11) - Raspberry Pi Storage
* [18 Gauge Wire](https://www.amazon.com/gp/product/B01LZRV0HV/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1) ($11) - Red and black power wires

### Purchased by Perez
* [Servo Driver HAT](https://www.pishop.us/product/servo-driver-hat-for-raspberry-pi-16-channel-12-bit-i2c/) ($18)- Buck converter from 3s Lipo 12V to 6V pi takes, makes control of servos and PWM control of motors less jittery
* [Raspberry Pi Cam V3](https://www.pishop.us/product/raspberry-pi-camera-module-3/) ($25)- Camera for driver in remote teleoperation
* [Blue LED ring switch](https://www.pishop.us/product/rugged-metal-on-off-switch-with-blue-led-ring-16mm-blue-on-off/) ($5) - On/Off switch
* [Mini Breadboard](https://www.pishop.us/product/mini-170-tie-points-breadboard/) ($2) - For wiring old RC transmitter circuit to new Pi circuit
* [Breadboard-frieldly Switch](https://www.pishop.us/product/breadboard-friendly-spdt-slide-switch/) ($1 x 2) - Switching between 2.4G RC control and Pi Control
* [M to F Jumper Cables](https://www.pishop.us/product/male-to-female-jumper-cable-x-40-20cm/) ($2.50) - Wiring RC Control and Pi Control

**Price with shipping: $68.72**

## Resources I used for the project:
[Raspberry Pi RC Car Wiring Tutorial](https://www.youtube.com/watch?v=mrvgF3CD1p4)
[Raspberry PI Servo HAT Documentation](https://www.waveshare.com/wiki/Servo_Driver_HAT)

## Wiring and hardware
!TODO: Give instructions on how to wire

## Setting up Software
1. Flash SD card using [Raspberry Pi Imager](https://www.raspberrypi.com/software/)
  - Select Raspberry Pi OS (other) -> Raspberry Pi OS Lite (64-bit)
  - In bottom right corner, select settings icon. Set hostname, enable ssh with password authentification, and set username/password. If planning to run off home wifi, configure wireless LAN. 
2. Connect to your raspberry pi to your desktop computer via ethernet, open a terminal, use the ssh command to connect to the pi
  - For example, for my pi, my hostname is raspberrypi.local and my username is cam, so I would type `ssh cam@raspberrypi.local`
  - Note that if you configured wireless LAN in the previous step and your desktop is on the same wifi you set up, then you do not need to connect via ethernet
4. Now, you should see a terminal prompt that looks like the following:
  - `cam@raspberrypi:~ $`
  - Congrats, you did it! We have a working computer for our little RC car. Now let's get him set up. First, we need to install the software to control the motors.
5. As a good practice, before installing anything, we should make sure our pi is up to date. Run the following command to look for updates and install them (this should take a while):
  - `sudo apt update && sudo apt upgrade -y` (the -y just tells linux to auto-accept the yes/no prompt, otherwise we have to manually press it, ew)
6. Now, let's enable the I2C interface. This will let us communicate with the servo hat that controls the motors. Run the following command:
  -  `sudo raspi-config`
  -  Using the arrows, choose Interfacing options -> I2C -> Yes
  -  Save changes, and run `sudo reboot` to restart pi and let changes take effect
7. Let's install the software libraries now so we can write Python to easily communicate with the servos.
  - `sudo apt-get install i2c-tools` useful package for checking if our servos are connected to the i2c interface of the Servo HAT
  - `sudo apt-get install python3-pip` installs python's tool for downloading python libraries
  - `sudo rm -rf /usr/lib/python3.11/EXTERNALLY-MANAGED` let's us use the base python installation instead of creating virtual environments (don't worry too much about this)
  - `pip3 install RPi.GPIO` package to let us use the GPIO pins on pi, requirements might already be satisfied
  - `sudo apt-get install python3-smbus` the magic sauce that let's us talk to the Servo HAT
8. Now's a good time to check the wiring on our servos. Run the following command:
  - `sudo i2cdetect -y 1`
If your output looks like this:
```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: 40 -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: 70 -- -- -- -- -- -- --
```
congrats! it looks like your servos are wired correctly
8. Alright, finally! It's time to get to the good stuff, some coding! For a development environment, I recommend using VSCode, and installing the Remote-SSH extension. Then pressing the >< symbol in the bottom left, you can select "connect to remote-ssh", enter your ssh command, and boom! You're now able to program on your raspberry pi just like you would with your desktop computer

