
ARUCO / VISP Hand-Eye Calibration on Baxter
=================================

![rviz wam screenshot](doc/aruco_hand_eye_wam.png)

## Use Cases

This package uses the ARUCO planar target tracker from `aruco_ros` and the VISP
hand-eye calibration from `visp_hand2eye_calibration` to provide a simple
camera pose estimation package.

If you're unfamiliar with Tsai's hand-eye calibration [1], it can be used in two ways:

- **eye-in-hand** -- To compute the static transform from a robot's
  end-effector to the optical frame of a camera. In this case, the camera is
  mounted on the end-effector, and you place the visual target so that it is
  fixed relative to the base of the rboot.
- **eye-on-base** -- To compute the static transform from a robot's base to the
  optical frame of a camera. In this case, the camera is mounted to the base of
  the robot (or kinematic chain), and you place the visual target so that it is
  fixed relative to the end-effector of the robot.

## Usage

For both use cases, you can either launch the `aruco_hand_eye.launch`
launchfile, or you can include it in another launchfile as shown below. Either
way, the launchfile will bring up the `aruco_ros` tracker and the
`visp_hand2eye_calibration` solver, along with an integration script. By
default, the integration script will interactively ask you to accept or discard
each sample.

### eye-in-hand

```xml
<launch>
  <include file="$(find aruco_hand_eye)/launch/aruco_hand_eye.launch">
    <arg name="markerid"   value="582"/>
    <arg name="markersize" value="0.150"/>
    <arg name="publish_tf" value="true"/>

    <arg name="marker_parent_frame" value="/base"/>
    <arg name="camera_parent_frame" value="/right_hand"/>

    <arg name="camera" value="/camera/rgb"/>
    <arg name="camera_frame" value="/camera_rgb_optical_frame"/>
  </include>
</launch>
```

### eye-on-base

```xml
<launch>
  <include file="$(find aruco_hand_eye)/launch/aruco_hand_eye.launch">
    <arg name="markerid"   value="582"/>
    <arg name="markersize" value="0.150"/>
    <arg name="publish_tf" value="true"/>

    <arg name="marker_parent_frame" value="/right_hand"/>
    <arg name="camera_parent_frame" value="/base"/>

    <arg name="camera" value="/camera/rgb"/>
    <arg name="camera_frame" value="/camera_rgb_optical_frame"/>
  </include>
</launch>
```
You also need to change the parameters below.

### aruco_hand_eye.launch
```xml

  <!-- Transform from the camera base link to the optical link (only necessary
       if you want aruco_hand_eye to compute the transform for you) -->
  <arg name="xyz_optical_base" default="[0.0, 0.022, 0.0]"/>
  <arg name="rpy_optical_base" default="[-1.571, -0.000, -1.571]"/>

```
Because when we do calibration, we want to get the relations between `camera_link` and `base`. But actually, camera  see pictures though `camera_rgb_optical _frame`. So we need to know the relations between  `camera_link` and `camera_rgb_optical _frame` using tf as well.

### Where should we put the marker on when doing calibration?
You can put it on anywhere relative to the last joint frame. 
 
### How to publish the tranformation between base and camera_link?
##### camera locate at top position
```xml
<node pkg="tf" type="static_transform_publisher" name="pub_tf_baxterBase2upcameraLink" args=" 0.652570 -0.093530 0.800550  0.030955 0.654864 -0.022162 0.754787  /base /camera_link 100 " />
```
##### camera locate at base position
```xml
<node pkg="tf" type="static_transform_publisher" name="pub_tf_baxterBase2cameraLink" args=" 0.200839 0.005365 0.290480 0.026372 0.029066 0.013821 0.999134  /base /camera_link 100 " />
```
# References

[1] *Tsai, Roger Y., and Reimar K. Lenz. "A new technique for fully autonomous
and efficient 3D robotics hand/eye calibration." Robotics and Automation, IEEE
Transactions on 5.3 (1989): 345-358.*