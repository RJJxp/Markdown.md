# 参数说明

static_transform_publisher x y z yaw pitch roll frame_id child_frame_id period_in_ms

Publish a static coordinate transform to tf using an 

x/y/z offset in meters 

yaw/pitch/roll in radians. 

yaw is rotation about Z

pitch is rotation about Y

roll is rotation about X

The period, in milliseconds, specifies how often to send a transform. 100ms (10hz) is a good value.