## CONNECTION

there are 4 connections on the radar, we only use 2 of them 

put the front side of the radar facing to you

the leftmost connection is for data transport. It uses cable(net wire), the green one.

the rightmost connection is for battery or charging. It uses black wire.

## INSTALLATION

git from the webset

https://github.com/clearpathrobotics/lms1xx.git

in the directory "~/catkin_ws/src"

open a new terminal, input:

> cd ~/catkin_ws
>
> catkin_make
>
> echo "source ~/catkin_ws/devel/setup.bash" >>~/.bashrc
>
> source ~/.bashrc

## NETWORK SETTING

as mentioned above
the radar uses cable(net wire) to transport the data
so you have to make sure your PC can find the radar in the net
which means your PC and the radar in the same segement

it requires the third number of your PC and radar's IP to be same
depending on the type of radar
usually the radar's IP is 192.168.0.1 or 192.168.1.121
so you have to change your PC's IP to 192.168.0.x or 192.168.1.y
x != 1 and y != 121

**right now this radar's IP is 192.168.1.121**

set up IP in Ubuntu
setting-->network-->wired-->option-->ipv4 setting
address:    192.168.1.5(we take x as 5)
newmask:    255.255.255.0
gateway:    0.0.0.0

after the setup
try to ping the radar's IP to see whether we could connect it
open a new terminal,input:

> ping 192.168.1.121

if connection is successful, we are ready to run the node

this part is according to webset:
https://blog.csdn.net/u013453604/article/details/50725833
https://www.cnblogs.com/21207-iHome/p/8022512.html

## RUN

open a new terminal, input:

> roscore

open a new ternimal, input:

> rosrun lms1xx LMS1xx_node _host:=192.168.1.121

to see the raw data, open a new terminal, input:

> rostopic echo /scan

to visaulize the raw data, open a new terminal, input:

> rviz

in the rviz window, add a **laserscan**, remeber the **frame_id** is "laser", and the **topic** is "/scan"

## ATTENTION

1. initialization

   when the radar is started, you should wait until the initialization is finished

   and there is no signal to judge that

   because all the light on the front side of radar won't blink

   only when the radar work **abnormally**, the light will be on​ 

2. wired and wireless net connection

   - conflict

     when the **amoudo** is using the both net connections, there will be a conflict sometimes.

     For instance, the IP of the radar we got is *192.168.1.121*, it uses wired connection so in order to find the radar in the amoudo, we have to set the amoudo's IP to be *192.168.1.x*,

     but in the wireless net connection, when you use **ROS** system, master and subordinate to have a remote control of the turtlebot , the IPs of the amoudo(master) and subordinate are also starts with *192.168.1.x*

     as a result of that, when you run the node mentioned in **RUN** part, the amoudo does not know which IP you mean. Here is the conflict.

   - solution

     when you know the reason why the conflict happens, it's easy to fix it.

     you have to change the amoudo's IP in wired connection **or** wireles connection.

     also like what mentioned in **NETWORK** **SETTING**, you could change wireless connection IP to be something like *192.168.2.x*, which means you also have to change the master and subordinate setup as the same time