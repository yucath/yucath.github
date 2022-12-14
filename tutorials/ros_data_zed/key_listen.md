# Node to listen key press

************************key_press.py************************

```
#!/usr/bin/env python3

import getch
import rospy
from std_msgs.msg import String #String message 
from std_msgs.msg import Int8

def keys():
    pub = rospy.Publisher('key',String,queue_size=10) # "key" is the publisher name
    rospy.init_node('keypress',anonymous=True)
    rospy.loginfo('Waiting for key')
    while not rospy.is_shutdown():
        #k=getch.getch()#
        k = input('Enter t and tree number for tree    ')
 
        if (k[0] == 't'):
            rospy.loginfo(k)
            pub.publish(k)#to publish

if __name__=='__main__':
    try:
        keys()
    except rospy.ROSInterruptException:
        pass
```