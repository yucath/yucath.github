# Launch file to start all nodes

**collect_data.launch**

```
<?xml version="1.0"?>

<launch>

<include
file="$(find zed_wrapper)/launch/zed2i.launch">

</include>

<node type="rviz" name="rviz" pkg="rviz" args="-d $(find zed_wrapper)/scan.rviz" />

<node pkg="zed_wrapper" type="[svo5.py](http://svo5.py/)" name="svo_node" output="screen" launch-prefix="gnome-terminal --command" />

<node pkg="zed_wrapper" type="key_press.py" name="keypress_node" output="screen" launch-prefix="gnome-terminal --command" />

<node pkg="zed_wrapper" type="bag_record_all.py" name="bagrecorder_node" output="screen" launch-prefix="gnome-terminal --command" />

<node pkg="reach_ros_node" type="nmea_tcp_driver2.py" name="reach_node" respawn="true" />

</launch>
```

create ‘**********scan.rviz’********** and add to ***/zed_wrapper/***

```jsx
Panels:
  - Class: rviz/Displays
    Help Height: 0
    Name: Displays
    Property Tree Widget:
      Expanded:
        - /Global Options1
        - /Status1
        - /Image1
      Splitter Ratio: 0.5
    Tree Height: 447
  - Class: rviz/Selection
    Name: Selection
  - Class: rviz/Tool Properties
    Expanded:
      - /2D Pose Estimate1
      - /2D Nav Goal1
      - /Publish Point1
    Name: Tool Properties
    Splitter Ratio: 0.5886790156364441
  - Class: rviz/Views
    Expanded:
      - /Current View1
    Name: Views
    Splitter Ratio: 0.5
  - Class: rviz/Time
    Name: Time
    SyncMode: 0
    SyncSource: PointCloud2
Preferences:
  PromptSaveOnExit: true
Toolbars:
  toolButtonStyle: 2
Visualization Manager:
  Class: ""
  Displays:
    - Alpha: 0.5
      Cell Size: 1
      Class: rviz/Grid
      Color: 160; 160; 164
      Enabled: true
      Line Style:
        Line Width: 0.029999999329447746
        Value: Lines
      Name: Grid
      Normal Cell Count: 0
      Offset:
        X: 0
        Y: 0
        Z: 0
      Plane: XY
      Plane Cell Count: 10
      Reference Frame: <Fixed Frame>
      Value: true
    - Alpha: 1
      Autocompute Intensity Bounds: true
      Autocompute Value Bounds:
        Max Value: 10
        Min Value: -10
        Value: true
      Axis: Z
      Channel Name: intensity
      Class: rviz/PointCloud2
      Color: 255; 255; 255
      Color Transformer: RGB8
      Decay Time: 0
      Enabled: true
      Invert Rainbow: false
      Max Color: 255; 255; 255
      Min Color: 0; 0; 0
      Name: PointCloud2
      Position Transformer: XYZ
      Queue Size: 10
      Selectable: true
      Size (Pixels): 3
      Size (m): 0.009999999776482582
      Style: Flat Squares
      Topic: /zed2i/zed_node/point_cloud/cloud_registered
      Unreliable: false
      Use Fixed Frame: true
      Use rainbow: true
      Value: true
    - Class: rviz/Image
      Enabled: false
      Image Topic: /zed2i/zed_node/left/image_rect_color
      Max Value: 1
      Median window: 5
      Min Value: 0
      Name: Image
      Normalize Range: true
      Queue Size: 2
      Transport Hint: raw
      Unreliable: false
      Value: false
  Enabled: true
  Global Options:
    Background Color: 48; 48; 48
    Default Light: true
    Fixed Frame: base_link
    Frame Rate: 30
  Name: root
  Tools:
    - Class: rviz/Interact
      Hide Inactive Objects: true
    - Class: rviz/MoveCamera
    - Class: rviz/Select
    - Class: rviz/FocusCamera
    - Class: rviz/Measure
    - Class: rviz/SetInitialPose
      Theta std deviation: 0.2617993950843811
      Topic: /initialpose
      X std deviation: 0.5
      Y std deviation: 0.5
    - Class: rviz/SetGoal
      Topic: /move_base_simple/goal
    - Class: rviz/PublishPoint
      Single click: true
      Topic: /clicked_point
  Value: true
  Views:
    Current:
      Class: rviz/XYOrbit
      Distance: 3.2530407905578613
      Enable Stereo Rendering:
        Stereo Eye Separation: 0.05999999865889549
        Stereo Focal Distance: 1
        Swap Stereo Eyes: false
        Value: false
      Field of View: 0.7853981852531433
      Focal Point:
        X: -1.6689300537109375e-06
        Y: -4.172325134277344e-07
        Z: 0
      Focal Shape Fixed Size: true
      Focal Shape Size: 0.05000000074505806
      Invert Z Axis: false
      Name: Current View
      Near Clip Distance: 0.009999999776482582
      Pitch: 0.3202040493488312
      Target Frame: <Fixed Frame>
      Yaw: 3.008584499359131
    Saved:
      - Class: rviz/Orbit
        Distance: 19.453998565673828
        Enable Stereo Rendering:
          Stereo Eye Separation: 0.05999999865889549
          Stereo Focal Distance: 1
          Swap Stereo Eyes: false
          Value: false
        Field of View: 0.7853981852531433
        Focal Point:
          X: 0
          Y: 0
          Z: 0
        Focal Shape Fixed Size: true
        Focal Shape Size: 0.05000000074505806
        Invert Z Axis: false
        Name: Orbit
        Near Clip Distance: 0.009999999776482582
        Pitch: 0.38539814949035645
        Target Frame: <Fixed Frame>
        Yaw: 5.963579177856445
Window Geometry:
  Displays:
    collapsed: false
  Height: 660
  Hide Left Dock: false
  Hide Right Dock: true
  Image:
    collapsed: false
  QMainWindow State: 000000ff00000000fd0000000400000000000001db000001fafc020000000cfb0000001200530065006c0065006300740069006f006e00000001e10000009b0000005c00fffffffb0000001e0054006f006f006c002000500072006f007000650072007400690065007302000001ed0000034800000185000000a3fb000000120056006900650077007300200054006f006f02000001df000002110000018500000122fb000000200054006f006f006c002000500072006f0070006500720074006900650073003203000002880000011d000002210000017afb0000000a0049006d00610067006502000000e6000000550000044f00000323fb0000000a0049006d006100670065010000003b000000f00000000000000000fb0000000a0049006d00610067006502000003010000001b0000047200000306fb000000100044006900730070006c006100790073010000003b000001fa000000c700fffffffb0000002000730065006c0065006300740069006f006e00200062007500660066006500720200000138000000aa0000023a00000294fb00000014005700690064006500530074006500720065006f02000000e6000000d2000003ee0000030bfb0000000c004b0069006e0065006300740200000186000001060000030c00000261fb0000000a0049006d0061006700650100000241000000c600000000000000000000000100000100000002b9fc0200000003fb0000001e0054006f006f006c002000500072006f00700065007200740069006500730100000041000000780000000000000000fb0000000a00560069006500770073000000003b000002b9000000a000fffffffb0000001200530065006c0065006300740069006f006e010000025a000000b20000000000000000000000020000060c00000120fc0100000001fb0000000a00560069006500770073030000004e00000080000002e100000197000000030000037b0000003efc0100000002fb0000000800540069006d006501000000000000037b0000030700fffffffb0000000800540069006d006501000000000000045000000000000000000000019a000001fa00000004000000040000000800000008fc0000000100000002000000010000000a0054006f006f006c00730100000000ffffffff0000000000000000
  Selection:
    collapsed: false
  Time:
    collapsed: false
  Tool Properties:
    collapsed: false
  Views:
    collapsed: false
  Width: 891
  X: 961
  Y: 27
```