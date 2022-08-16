# ArduPilot Gazebo Simulator plugin
This is ArduPilot official plugin for Gazebo Simulator plugin
It replaces the old Gazebo plugin to bring support of the next gen Gazebo simulator.
It also brings support for more features :
- more flexible data exchange between SITL and Gazebo with JSON data sharing.
- more sensors supported.
- true Simulation lockstepping. It is now possible to use GDB to stop the Gazebo time for debugging.
- Better 3D rendering

The project is composed of a Gazebo plugin to connect to ArduPilot SITL (Software In The Loop) and some example models and worlds.

## Disclaimer :
The plugin is currently working, but we are working into bringing support for more feature and refine the API.

## Prerequisites :
Gazebo Sim Garden is supported on Ubuntu Focal and Jammy. If you are running Ubuntu as a virtual machine you will need at least Ubuntu 20.04 (Focal) in order to have the OpenGL support required for the `ogre2` render engine.

Follow the instructions for a [binary install of Gazebo Garden](https://gazebosim.org/docs/garden/install) and verify that Gazebo is running correctly.

Set up an [ArduPilot development environment](https://ardupilot.org/dev/index.html). In the following it is assumed that you are able to
run ArduPilot SITL using the [MAVProxy GCS](https://ardupilot.org/mavproxy/index.html).

## Installation :

Install Gazebo Sim Gazebo development libs and rapidjson:
````
sudo apt install rapidjson-dev gz-sim7-dev
````

Clone the repo and build with:
````bash
git clone https://github.com/ArduPilot/ardupilot_gazebo -b ignition-garden
cd ardupilot_gazebo
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=RelWithDebInfo
make -j4
````

## Running :

Set the Gazebo environment variables in your `.bashrc` or `.zshrc` or in  the terminal used to run gazebo:

### In terminal
Assuming that you have clone the repository in `$HOME/ardupilot_gazebo`:
```bash
export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/ardupilot_gazebo/build:$GZ_SIM_SYSTEM_PLUGIN_PATH
export GZ_SIM_RESOURCE_PATH=$HOME/ardupilot_gazebo/models:$HOME/ardupilot_gazebo/worlds:$GZ_SIM_RESOURCE_PATH
```

### In .bashrc
Assuming that you have clone the repository in `$HOME/ardupilot_gazebo`:
```bash
echo 'export GZ_SIM_SYSTEM_PLUGIN_PATH=$HOME/ardupilot_gazebo/build:${GZ_SIM_SYSTEM_PLUGIN_PATH}' >> ~/.bashrc
echo 'export GZ_SIM_RESOURCE_PATH=$HOME/ardupilot_gazebo/models:$HOME/ardupilot_gazebo/worlds:${GZ_SIM_RESOURCE_PATH}' >> ~/.bashrc
```

Reload your terminal with source ~/.bashrc

### Run Gazebo

```bash
gz sim -v 4 -r iris_arducopter_runway.world
```

The `-v 4` parameter is not mandatory, it shows the debug informations.

### Run ArduPilot SITL
To run an ArduPilot simulation with Gazebo, the frame should have `gazebo-` in it and have `JSON` as model. Other commandline parameters are the same as usal on SITL.
```bash
sim_vehicle.py -v ArduCopter -f gazebo-iris --model JSON --map --console
```

### Arm and takeoff

```bash
STABILIZE> mode guided
GUIDED> arm throttle
GUIDED> takeoff 5
```

## Troubleshooting

### Gazebo issues
Gazebo documentation is already providing some help on most frequents issues https://gazebosim.org/docs/garden/troubleshooting#ubuntu
