# Node to count frames

**frame_count.py**

```
#! /usr/bin/env python3

import rospy
from sensor_msgs.msg import Image
from std_msgs.msg import Int32
import numpy as np

class counter:

    def __init__(self):
        self.counts = 0
        self.pub = rospy.Publisher('frame_count',Int32,queue_size=30)
        self.sub = rospy.Subscriber("/zed2i/zed_node/right/image_rect_color", Image, self.callback, queue_size=30)

    def callback(self,data):
        self.counts = self.counts+1
        rospy.loginfo(self.counts)
        msg = self.counts
        self.pub.publish(msg)

def main():
    '''Initializes and cleanup ros node'''
    c = counter()
    rospy.init_node('frame_counter')

    try:
        rospy.spin()
    except KeyboardInterrupt:
        print ("Shutting down ROS Image feature detector module")

if __name__ == "__main__":
    main()
```