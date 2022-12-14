# Node to record GPS data

```jsx
#! /usr/bin/env python3

# Software License Agreement (BSD License)
#
# Copyright (c) 2013, Eric Perko
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
#  * Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
#  * Redistributions in binary form must reproduce the above
#    copyright notice, this list of conditions and the following
#    disclaimer in the documentation and/or other materials provided
#    with the distribution.
#  * Neither the names of the authors nor the names of their
#    affiliated organizations may be used to endorse or promote products derived
#    from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
# CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
import time
import serial
import socket
import rospy
import os
import sys
from nmea_msgs.msg import Sentence

class ReachSocketHandler:

    # Set our parameters and the default socket to open
    def __init__(self,host,port):
        self.time1 = time.time()
        self.host = host
        self.port = port
        self.pub = rospy.Publisher('/reach_gps_message', Sentence, queue_size=10)

    # Should open the connection and connect to the device
    # This will then also start publishing the information
    def start(self):
        # Try to connect to the device
        rospy.loginfo('Connecting to Reach RTK %s on port %s' % (str(self.host),str(self.port)))
        self.connect_to_device()
        try:
            # Create the driver
            while not rospy.is_shutdown():
                #GPS = soc.recv(1024)
                data = self.buffered_readLine().strip()
      #          if '$GPGGA' not in data:
        #            data = ','.join(['$GPGGA'] + data.split())
      #          status = data.split(',')[6]
        #        rospy.loginfo(data)
      #          if status == '0':
       #             rospy.logwarn_throttle(1.0, '[REACH] Status 0, GPS doesn\'t have lock!')
      #          elif status == '4':
       #             rospy.loginfo_throttle(10.0, "[REACH] Have RTK Fix (4)")
      #          elif status == '5':
      #              rospy.loginfo_throttle(10.0, "[REACH] Have RTK Float (5)")
       #         else:
     #               rospy.loginfo_throttle(1.0, "[REACH] Got non-RTK message status {}".format(status))

                # Debug print message line
                data1 = data.split('$')[1]
                time_prev = self.time1
                self.time1 = time.time()
                # print(self.time1 - time_prev)
                # print(data)
                # print(data1)
                # print('hi')
                #
                m = data[3]+data[4]+data[5]

                
                if m == 'GGA':
                    # print(data)
                    msg = Sentence()
                    msg.sentence = data
                
                    msg.header.stamp = rospy.Time.now()
                    self.pub.publish(msg)
        except rospy.ROSInterruptException:
            # Close GPS socket when done
            self.soc.close()

    # Try to connect to the device, allows for reconnection
    # Will loop till we get a connection, note we have a long timeout
    def connect_to_device(self):
        while not rospy.is_shutdown():
            try:
                self.soc = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                self.soc.settimeout(5.0)
                self.soc.connect((self.host,self.port))
                rospy.loginfo('Successfully connected to device, starting publishing!')
                return
            except socket.timeout:
                rospy.logwarn_throttle(30,'Socket connection timeout. Retrying...')
                continue
            except Exception as e:
                rospy.logerr("Socket connection error. Error was: %s." % e)
                exit()

    # Try to connect to the device, assuming it just was disconnected
    # Will loop till we get a connection
    def reconnect_to_device(self):
        rospy.logwarn('Device disconnected. Reconnecting...')
        self.soc.close()
        self.connect_to_device()

    # Read one line from the socket
    # We want to read a single line as this is a single nmea message
    # https://stackoverflow.com/a/41333900
    # Also set a timeout so we can make sure we have a valid socket
    # https://stackoverflow.com/a/15175067
    def buffered_readLine(self):
        line = ""
        while not rospy.is_shutdown():
            # Try to get data from it
            try:
                part = self.soc.recv(1)
            except socket.timeout:
                self.reconnect_to_device()
                continue
            # See if we need to process the data
            if not part or len(part) == 0:
                self.reconnect_to_device()
                continue
            #if part != "\n":
            #    line += part
            #elif part == "\n":
            if part.decode() != "\n":
                line += part.decode()
            else:
                break
        return line

if __name__ == '__main__':

    # Initialize our ros node
    rospy.init_node('reach_ros_node')

    # Read in ROS parameters
    host = rospy.get_param('~host', '192.168.42.1')
    port = rospy.get_param('~port', 9001)

    # Open the socket to our device, and start streaming the data
    device = ReachSocketHandler(host,port)
    device.start()
```