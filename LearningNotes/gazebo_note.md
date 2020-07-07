# Gazebo Learning Notes

 [Official tutorial](http://gazebosim.org/tutorials) 

Recite the knowledge in my WORDS.

[toc]

## 00. Problems encountered

- Run gazebo, problem like [this](https://blog.csdn.net/qq_43802597/article/details/97996255) .

- Can not load world file , the Gazebo goes black.

  Gazebo can not find the model used in world file.



## 01. Base

### 01-01. Common use of `gazebo`

The `Gazebo ` command actually runs 2 different executables. First is `gzserver` , second is `gzclient` .

The `gzserver` executable runs the physics update-loop and sensor data generation. 

```bash
gzserver <world_filename>	# load the world file
gzserver -s <plugin_filename> # load the plugin, plugin is always a libXXX.so
```

The `gzclient` executables runs a QT based user interface.

```bash
gzclient --gui-client-plugin <plugin_filename> # load the plugin, plugin is always a libXXX.so
```

- Load the  world files

  > gazebo worlds/pioneer2dx.world

### 01-02. Short Introduction to World Files and Model Files

- World files located in `/usr/share/gazebo-9/worlds` , world files search paths are below:

  1. relative to current directory
  2. an absolute path
  3. relative to a path component in `GAZEBO_RESOURCE_PATH` 
  4. `worlds/<world_name>` , where \<world_name\> is installed with Gazebo

- world files contains all the elements in a simulation, including robots, lights, sensors and static objects by \<include\> tag of model files.

  ```xml
  <include> 
      <uri>model://model_file_name</uri>
  </include>
  ```

- Model files and world file both use SDF(Simulation Description Format). World files typically have a `.world` extension. Model files have a `.sdf` extension. Model files can facilitate reuse of models and simplify world files.

- Here are some Gazebo's environment variables

  `GAZEBO_MODEL_PATH` , `GAZEBO_RESOURCE_PATH` , `GAZEBO_MASTER_URI` , `GAZEBO_PLUGIN_PATH`,  `GAZEBO_MODEL_DATABASE_URI` 

  ```bash
  # use default setting or check it
  source usr/share/gazebo/setup.sh
  ```

### 01-03. Gazebo Distributed Architecture

The whole system include 7 parts: 

1. Gazebo master: topic management

2. communication library: bridge between server and client

3. physics library, rendering library, sensor generation: mainly server's job
4. GUI, plugins: mainly client's job

- gzserver: simulate the physics, rendering and sensors
- gzclient: provide a graphical interface to visualize and interact



## 02 Build a Robot

### 02-01. Model Structure and requirments

- Model database structure 

- Single model folder structure

  plugins, meshes, material, model config(name, version, sdf, author, description, depend), model sdf

### 02-02. Make a Model

 [Clike Here](http://gazebosim.org/tutorials?tut=build_model&cat=build_robot) 

SDF models has 3 main parts: links, joints and plugins.

- links: collision, visual, inertial, sensor, light

  For example, a table model could consist of 5 links (4 for the legs and 1 for the top) connected via joints. However, this is overly complex, especially since the joints will never move. Instead, create the table with 1 link and 5 collision elements.

- joints: connects two links
- plugins: a shared library created by a third party to control a model

Here is an example of a box:

```xml
<?xml version='1.0'?>
<sdf version="1.4">
  <model name="my_model">
    <pose>0 0 0.5 0 0 0</pose>
    <static>true</static>
    <link name="link">
      <inertial>
        <mass>1.0</mass>
        <inertia> <!-- inertias are tricky to compute -->
          <!-- http://gazebosim.org/tutorials?tut=inertia&cat=build_robot -->
          <ixx>0.083</ixx>       <!-- for a box: ixx = 0.083 * mass * (y*y + z*z) -->
          <ixy>0.0</ixy>         <!-- for a box: ixy = 0 -->
          <ixz>0.0</ixz>         <!-- for a box: ixz = 0 -->
          <iyy>0.083</iyy>       <!-- for a box: iyy = 0.083 * mass * (x*x + z*z) -->
          <iyz>0.0</iyz>         <!-- for a box: iyz = 0 -->
          <izz>0.083</izz>       <!-- for a box: izz = 0.083 * mass * (x*x + y*y) -->
        </inertia>
      </inertial>
      <collision name="collision">
        <geometry>
          <box>
            <size>1 1 1</size>
          </box>
        </geometry>
      </collision>
      <visual name="visual">
        <geometry>
          <box>
            <size>1 1 1</size>
          </box>
        </geometry>
      </visual>
    </link>
  </model>
</sdf>
```

A good order to add features:

1. Add a link
2. Set the collision element
3. Set the visual element
4. Set the inertial properties
5. Go to 1. until all links have been added
6. Add all joints(if any)
7. Add all plugins(if any)

### 02-03. Make a Mobile Robot

 [Click Here](http://gazebosim.org/tutorials?tut=build_robot&cat=build_robot) 

Check these heavy steps on the link above

encounter a warning like:

```bash
Node::Advertise(): Error advertising a topic. Did you forget to start the discovery service?
```

- [import meshes ](http://gazebosim.org/tutorials?tut=import_mesh&cat=build_robot)  
- [attach meshes](http://gazebosim.org/tutorials?tut=attach_meshes&cat=build_robot) 





