## Install Readme

This if for the total install and setup of Pluto robot with a Logitech F710 gamepad joystick controller.

Some notes:
* The drivers (and ROS node) for the ATRV Pluto robot are written by John Folkesson, who modified the [ATRV Jnr Drivers](https://github.com/diasdm/rflex) originally written by David V. Lu & Mikhail Medvedev.
* The proprietary board for which the drivers have been written is an rFlex 2.2.5. For completeness this is the [rFlex Manual](https://cse.buffalo.edu/~shapiro/Courses/CSE716/MobilityManRev4.pdf), although it does not appear useful at this time.


## Dependencies & Build

Dependencies include the ROS joy, costmap_2d and navigation stack packages. _Make sure to list if some are missing_

Install the dependencies
```bash
sudo apt update && sudo apt-get install ros-noetic-joy ros-noetic-navigation ros-noetic-costmap-2d
```

Clone this repository **update repo link if it changes**:
```bash
cd ~/catkin_ws/src
git clone https://gits-15.sys.kth.se/magn2/rpl_PlutoATRV.git
```

Build the packages:
```bash
cd ~/catkin_ws
catkin build
```

## Hardware Setup

### Wireless Controller

One should refer to [ROS Joy](http://wiki.ros.org/joy) and [Configuring and Using a Linux-Supported Joystick with ROS](http://wiki.ros.org/joy/Tutorials/ConfiguringALinuxJoystick) to ensure that the wireless controller being used is working as is desired.

### Serial and Wireless Controller Communication

Connect the wireless reciever of the controller and the USB serial connection to Pluto.
```bash
# check the usb connections
lsusb
```

Should see the following or similar:
```bash
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 012: ID 046d:c21f Logitech, Inc. F710 Wireless Gamepad [XInput Mode] # the wireless controller
Bus 001 Device 014: ID 0403:6001 Future Technology Devices International, Ltd FT232 Serial (UART) IC # Pluto
```

The serial port is typically /dev/ttyUSB0. Check the serial ports by running:
```bash
dmesg | grep tty
```

Give the following permissions to the serial port by running:
```bash
sudo chmod a+rw /dev/ttyUSB0
```

Run the following (if vim not installed, use nano or some other editor):
```bash
sudo vim /etc/udev/rules.d/10-colca.rules
KERNEL=="js*", SUBSYSTEM=="input", ATTRS{idVendor}=="046d", ATTRS{idProduct}=="c21f", MODE:="0777",SYMLINK+="logitech_joy"
KERNEL=="ttyUSB*", ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", MODE:="0777",SYMLINK+="pluto_serial"
service udev reload
service udev restart
# important step!! plugin out and in again!
```

List USB devices again.
```bash
ls -l /dev | grep -E "logitech|pluto"
```

Which should display the following.
```bash
lrwxrwxrwx   1 root    root             9 aug  9 14:57 logitech_joy -> input/js0
lrwxrwxrwx   1 root    root             7 aug  9 14:56 pluto_serial -> ttyUSB0
```
## Connecting ROS via LAN
The machines mounted on Pluto communicate between each other throught Pluto´s own LAN network. To setup the communication
1. Connect each machine to **pluto-net** (Password: *robot-pluto*)
2. Run *ifconfig* and look at the *wlp2s0* to find each machine´s IP address\
For example's purposes, let's assume to have two machines\
1<sup>st</sup> machine: 192.168.1.205 (intel nuc, running **ros-master**)\
2<sup>nd</sup> machine: 192.168.1.84
3. On the first machine run the command `nano .bashrc` and add the following lines
```
export ROS_MASTER_URI=http://192.168.1.205:11311
export ROS_IP=192.168.1.205
```
Then save the file and source it with
```
. ~/.bashrc
```
4. On the second machine run the command `nano .bashrc` and add the following lines
```
export ROS_MASTER_URI=http://192.168.1.205:11311
export ROS_IP=192.168.1.84
```
Then save the file and source it with
```
. ~/.bashrc
```
5. To test if the communection to the core works properly, one can use the command `rostopic`
```
rostopic list
```
If connection works propertly, running this one any machine connected to the core should return a list of all topics available, if not an error is returned.


## Issues
1. Building the pluto package for the first time:
    1. After cloning and then building the pluto package for the first time there might be an error pertaining to a missing include folder.
    ```bash
    CMake Error at /opt/ros/noetic/share/catkin/cmake/catkin_package.cmake:305 (message):
    catkin_package() include dir 'include' does not exist relative to 
    '/home/laptop-02/catkin_ws/src/rpl_PlutoATRV/pluto'
    ```
    2. It can be addressed by manually creating the **include** folder inside the `.../rpl_PlutoATRV/pluto/` directory.
    

