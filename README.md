## KTH-RPL Pluto ATRV Robot

Why do we need this?

1. The iRobot ATRV was released in 2007, and as such support from iRobot is no longer offered. This repo provides drivers and instructions for setting up and running Pluto with the use of a wireless controller.
2. The goal is to utilize Pluto for data collection in an outdoor/rugged environent.
3. This specific readme only details how to **run** Pluto. For installation and configuration please refer to the [install readme](https://github.com/migsdigs/iRobotATRV/blob/main/assets/install/readme.md).
4. The wireless controller used is the Logitech F710 Wireless Gamepad. Refer to [install readme](https://github.com/migsdigs/iRobotATRV/blob/main/assets/install/readme.md) for its setup.

Test system: Ubuntu 20.04, ROS Noetic

Author: Miguel Garcia Naude

---

Sensor setup:
* **add later on**

Front-Side    | Top-Back
------------- | -------------
![picture alt](https://github.com/migsdigs/iRobotATRV/blob/main/assets/img/front.jpg "Front")  | ![picture alt](https://github.com/migsdigs/iRobotATRV/blob/main/assets/img/back.jpg "Back")


## Power On
1. At the back of the robot, flick the grey switch from left to right (main power switch).
2. On top, towards the back of the robot, press and release the knob on the right of the display, this will power on the robot propriety rFlex compute.
3. Using the knob, scroll to setting labelled **Brake**, push the knob down, and ensure that the screen displays **Brake: off** in the bottom right.


## Power Off
1. Shutdown the NUC (from terminal: `sudo poweroff`)
2. Using the knob and display at the back of the robot, scroll to **PWR** and push the knob.
3. Proceed to scroll to **Shutdown PC** and push the knob.
4. Flick the grey switch at the back of the robot from right to left.

## Build & Run

Note again: see [install readme](https://github.com/migsdigs/iRobotATRV/blob/main/assets/install/readme.md) for installation instructions

The path to the ATRV drivers and Pluto code on the NUC is:

```bash
atrv-nuc@atrvnuc-desktop /home/atrv-nuc/catkin_ws
```

### Build
```bash
cd ~/catkin_ws
catkin build
source devel/setup.bash
```

### Run
### Connecting to INTEL NUC
Connect your laptop to the network **pluto-net** 

In a new terminal:
```bash
ssh pluto-nuc@192.168.1.205
```
Enter the NUC's password (*robot*)

#### Running ROS program
In the terminal connected to the Intel NUC
```bash
roscore
```

In a new terminal:
```bash
roslaunch pluto pluto.launch
```

Should see:
```
[ INFO] [1691586146.958756627]: Attempting to connect to /dev/ttyUSB0 # pluto serial connection
[ INFO] [1691586146.959305916]: in initialize
[ INFO] [1691586146.969091000]: Connected!
```
And odometry information should begin posting to terminal.

#### Use of Wireless Controller
1. To begin using the wireless controller, press and release the **LB** button.
    1. This button acts as a handbrake and the robot will only recieve velocity commands after it is deactivated.
2. Use the **left pad** to control the robot.
3. Use the **LT** trigger to increase and degreas speed.


## Issues

Some common problems are addressed below.

1. Cannot open /dev/ttyUSB0: Permission denied #26 <br> [https://github.com/esp8266/source-code-examples/issues/26](https://github.com/esp8266/source-code-examples/issues/26)
2. Issues with serial communication <br> 
**add stuff here**


