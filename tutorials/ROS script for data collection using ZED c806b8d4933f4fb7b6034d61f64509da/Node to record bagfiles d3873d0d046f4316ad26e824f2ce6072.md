# Node to record bagfiles

**********************************bag_record_all.py**********************************

```jsx
#! /usr/bin/env python3

import os
import rospy

rospy.init_node('rosbag_node')
rospy.loginfo('started bag recording 1')

while True: 
    inp = input('\n Bagname: \n')

    path = "/home/achyut/Data_FALL_2022/week8/bagfiles/"
    bagname = path+inp+'.bag'

    os.system("rosbag record -O " +bagname+" /zed2i/zed_node/imu/data /zed2i/zed_node/pose /zed2i/zed_node/odom /zed2i/zed_node/path_odom /reach_gps_message /key /frame_count")
```