# How to do camera intrinsic calibration

Hi  guys, Here is a tutorial about how to do intrinsic calibration

 * Before you do camera intrinsic calibration, make sure you already pluged into camera into your computer.
 * If the screen show up like below, which means you have linked camera succesfully.
```javascript
 [ INFO] [1511487551.563886120]: Initializing nodelet with 4 worker threads.
 [ INFO] [1511487553.491886139]: Device "1d27/0609@1/11" found.
Warning: USB events thread - failed to set priority. This might cause loss of data...
 [ INFO] [1511487557.144959648]: Starting color stream.
 [ INFO] [1511487557.282757437]: using default calibration URL
 [ INFO] [1511487557.283189230]: camera calibration URL: file:///home/tony/.ros/camera_info/rgb_PS1080_PrimeSense.yaml
```
 * But if the screen show like below, which means failed
 ```javascript
[ INFO] [1511500896.426104144]: Initializing nodelet with 4 worker threads.
[ INFO] [1511500897.480454757]: Device "1d27/0609@1/14" found.
[ INFO] [1511500897.486514182]: No matching device found.... waiting for devices. Reason: openni2_wrapper::OpenNI2Device::OpenNI2Device(const string&) @ /tmp/binarydeb/ros-indigo-openni2-camera-0.2.7/src/openni2_device.cpp @ 74 : Device open failed
	Could not open "1d27/0609@1/14": Failed to open the USB device!
```
## How to solve it?
```javascript
tony@Avatar:~$ lsusb
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 012: ID 1770:ff00  
Bus 001 Device 006: ID 17ef:6019 Lenovo 
Bus 001 Device 014: ID 1d27:0609 ASUS 
Bus 001 Device 010: ID 0bda:0129 Realtek Semiconductor Corp. RTS5129 Card Reader Controller
Bus 001 Device 009: ID 5986:014c Acer, Inc 
Bus 001 Device 013: ID 8087:0a2a Intel Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
tony@Avatar:~$ cd /dev/bus/usb/001/
tony@Avatar:/dev/bus/usb/001$ ls -l
total 0
crw-rw-r-- 1 root root 189,  0 11月 24 09:29 001
crw-rw-r-- 1 root root 189,  5 11月 24 09:29 006
crw-rw-r-- 1 root root 189,  8 11月 24 09:29 009
crw-rw-r-- 1 root root 189,  9 11月 24 09:29 010
crw-rw-r-- 1 root root 189, 11 11月 24 12:55 012
crw-rw-r-- 1 root root 189, 12 11月 24 12:55 013
crw-rw-r-- 1 root root 189, 13 11月 24 13:21 014
tony@Avatar:/dev/bus/usb/001$ sudo chmod 666 014

crw-rw-r-- 1 root root 189,  0 11月 24 09:29 001
crw-rw-r-- 1 root root 189,  5 11月 24 09:29 006
crw-rw-r-- 1 root root 189,  8 11月 24 09:29 009
crw-rw-r-- 1 root root 189,  9 11月 24 09:29 010
crw-rw-r-- 1 root root 189, 11 11月 24 12:55 012
crw-rw-r-- 1 root root 189, 12 11月 24 12:55 013
crw-rw-rw- 1 root root 189, 13 11月 24 13:21 014


```
What we need to do is change the AUSU's mode to let it has write ability. you can see in the last coloum.


 * Then look at the two websites,which can give you a guidance. Please be careful with the parameters! If you can not use openni, you can use openni2 instead.
  
     http://wiki.ros.org/camera_calibration/Tutorials/MonocularCalibration
     http://wiki.ros.org/openni_launch/Tutorials/IntrinsicCalibration

## where should we put the .yaml file in?
.ros/camera_info