# Node to Record SVO

**svo5.py**

```jsx
#! /usr/bin/env python3

from operator import imul
import sys
import rospy
from zed_interfaces.srv import *
from sensor_msgs.msg import Imu
from sensor_msgs.msg import Image
from std_msgs.msg import String
from geometry_msgs.msg import PoseStamped
from nav_msgs.msg import Path
from nav_msgs.msg import Odometry
from nmea_msgs.msg import Sentence
import rosbag
from std_msgs.msg import Int32
import os 
from subprocess import call

path = "/home/achyut/Data_FALL_2022/week8/"
rospy.init_node('svo_client',anonymous=True)
rospy.loginfo('started svo recording 1')
rospy.wait_for_service('/zed2i/zed_node/start_svo_recording')

class recorder():

    def __init__(self,bagname):
        self.counts = 0
        self.tree = 0
        self.pub = rospy.Publisher('frame_count',Int32,queue_size=30)
        self.sub6 = rospy.Subscriber("/zed2i/zed_node/right/image_rect_color", Image, self.framecounter, queue_size=30)
        self.sub5 = rospy.Subscriber("/key", String, self.key,queue_size=10)

        print('\nfinished subscribing')
        print(bagname)
        

    def key(self,data):
        self.tree += 1
        print('written {}'.format(self.tree))
        

    def framecounter(self,data):
        self.counts = self.counts+1
        num = Int32()
        num.data = self.counts
        msg = self.counts
        self.pub.publish(msg)

topic1 = ' /zed2i/zed_node/imu/data '
topic2 = '/zed2i/zed_node/pose '
topic3 = '/zed2i/zed_node/odom '
topic4 = '/zed2i/zed_node/path_odom '
topic5 = '/reach_gps_message '
topic6 = '/key '
topic7 = '/frame_count '

while True:
    inp = input('\nEnter z to start, space to stop and q to quit:\n')
    
    if inp == 'q':
        break

    elif inp == 'z':
        file_name = input('\n Enter the svoname: \n')

        svoname = path+'svo/'+file_name+'.svo'
        bagname = path+'bagfiles/'+file_name+'.bag'
        # bag = rosbag.Bag(bagname,'w')

        sta = rospy.ServiceProxy('/zed2i/zed_node/start_svo_recording',start_svo_recording)
        sta(svoname)
        
        a = recorder(bagname)
        rospy.loginfo("\n started recording\n")
        
        while True:

            inp1 = input("\n Enter space to stop recording: \n")
            if inp1 == ' ':
                stp = rospy.ServiceProxy('/zed2i/zed_node/stop_svo_recording',stop_svo_recording)
                stp()
                a.sub6.unregister()
                break
        

    else:
        rospy.loginfo('\nEnter only z or space')
```