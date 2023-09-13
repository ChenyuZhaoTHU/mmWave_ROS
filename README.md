
---
### Quick start guide (AWR1642BOOST ES2.0 EVM):
1. Mount AWR1642BOOST ES2.0 EVM (as below), connect 5V/2.5A power supply and connect a micro-USB cable to host Ubuntu 16.04 LTS with [ROS Kinetic](http://wiki.ros.org/kinetic).
   
![](https://github.com/radar-lab/ti_mmwave_rospkg/raw/master/auxiliary/mounting.jpg "AWR1642 Mounting")

Note: Tested with Ubuntu 16.04 LTS with ROS Kinectic and Ubuntu 18.04 LTS with [ROS Melodic](http://wiki.ros.org/melodic)

2. Download SDK 2.0 or above (suggested SDK 2.1) from [here](http://www.ti.com/tool/MMWAVE-SDK) and use [UNIFLASH](http://www.ti.com/tool/UNIFLASH) to flash xwr16xx_mmw_demo.bin to your device. **Do not forget SOP2 jumper when flashing.**

Note:
AWR1642 ES1.0 (usually purchased before May 2018) uses SDK 1.2. AWR1642 ES2.0 (usually purchased after May 2018) uses SDK 2.0. Same applies to AWR1443. (You can refer to [this thread](https://e2e.ti.com/support/sensors/f/1023/t/692195?tisearch=e2e-sitesearch&keymatch=%20user:356347))

3. Clone this repo and ROS serial onto your `<workspace dir>/src`:

```
git clone https://github.com/radar-lab/ti_mmwave_rospkg.git
git clone https://github.com/wjwwood/serial.git
```
4. Go back to `<workspace dir>`:

```
First, open the CMakeLists.txt at /catkin_ws/src/ti_mmwave_rospkg
change the line add_definitions(-std=c++11) to add_definitions(-std=c++14)
Then,
catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3 && source devel/setup.bash
echo "source <workspace_dir>/devel/setup.bash" >> ~/.bashrc
```

5. Enable command and data ports on Linux:
```
sudo chmod 666 /dev/ttyACM0
sudo chmod 666 /dev/ttyACM1
```
Note: If multiple sensors are used, enable additional ports `/dev/ttyACM2` and `/dev/ttyACM3`, etc. the same as this step.

6. Launch IWR1843 config:
```
roslaunch ti_mmwave_rospkg iwr1843_withcfg.launch
```

Note: If you want to build your own config, use [mmWave Demo Visualizer](https://dev.ti.com/mmwavedemovisualizer) and link the launch file to the config.

7. ROS topics can be accessed as follows:
```
rostopic list
rostopic echo /ti_mmwave/radar_scan
```
8. ROS parameters can be accessed as follows:
```
rosparam list
rosparam get /ti_mmwave/max_doppler_vel
```

Note: AWR1843 requires SDK 3.2.0.4, which has different output format. The later release will improve this part.

---
### Message format:
```
header: 
  seq: 6264
  stamp: 
    secs: 1538888235
    nsecs: 712113897
  frame_id: "ti_mmwave"   # Frame ID used for multi-sensor scenarios
point_id: 17              # Point ID of the detecting frame (Every frame starts with 0)
x: 8.650390625            # Point x coordinates in m (front from antenna)
y: 6.92578125             # Point y coordinates in m (left/right from antenna, right positive)
z: 0.0                    # Point z coordinates in m (up/down from antenna, up positive)
range: 11.067276001       # Radar measured range in m
velocity: 0.0             # Radar measured range rate in m/s
doppler_bin: 8            # Doppler bin location of the point (total bins = num of chirps)
bearing: 38.6818885803    # Radar measured angle in degrees (right positive)
intensity: 13.6172780991  # Radar measured intensity in dB
```
---
