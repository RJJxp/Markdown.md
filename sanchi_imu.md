### INSTALL THE DRIVER

https://cuizhouai.coding.net/p/tergeo-indoor/git/tree/master/src/sensors/imu


you can git the driver of the imu from the webset above

open the terminal, input:

> cd ~/catkin_ws
>
> catkin_make
>
> echo "source ~/catkin_ws/devel/setup.bash">> ~/.bashrc
>
> source ~/.bashrc

### RUN THE DRIVER

1. start the ros system in the terminal

> roscore

2. run the node by opening a new terminal

> rosrun sanchi_amov sanchi_amov _port:=/dev/ttyUSB0 _model:=100D2 _baud:=115200

  according to the parameters in   **~/catkin_ws/src/sanchi_amov/launch/imu100D2.launch**

if the port's permission is denied, then input the following commands in the terminal

> sudo chmod +x /dev/ttyUSB0
>
> sudo chmod 777 /dev/ttyUSB0

when you have already starts the node, it will publish the imu sensor data in the topic

**/imu/data_raw**

so if you want to see the raw data, use **rostopic** command like:

> rostopic echo /imu/data_raw
>
> rostopic hz /imu/data_raw