# Support

If you have difficulty following the steps in this document, please either contact Bruno (bruno@robots.ax.ac.uk) or Nick (nickh@robots.ox.ac.uk) or use the rocket chat channel at https://mrgrocket.oxfordrobots.com/channel/planning_tutorial

# Computer Setup 

To take part in the practical session of the tutorial, you will need to have your own laptop configured with the following software.

1. Install Ubuntu Linux 16.04LTS 64bit on your computer. Please make sure that you have exactly this version installed: 16.04 for 64bit. Download the image from here: http://releases.ubuntu.com/16.04/ (the image download is ubuntu-16.04.4-desktop-amd64.iso). Note, that you can perfectly install Ubuntu 16.04 alongside an existing operating system (even on a MacBook), or you could run this in a virtual machine.

2. Within Ubuntu, follow the instructions at https://github.com/LCAS/rosdistro/wiki to install both ROS Kinetic and the STRANDS software packages. We assume a basic understanding of Unix-like operating systems and shell usage here. If you need help using the command line, this might be a good start: https://help.ubuntu.com/community/UsingTheTerminal. 
The relevant installation steps are copied below for your convenience:
    1. Enable the ROS repositories: Follow steps 1.1-1.3 in http://wiki.ros.org/kinetic/Installation/Ubuntu. There is no need to do the steps after 1.3. In short, this is:
        1. Configure your Ubuntu repositories to allow "restricted," "universe," and "multiverse."
        2. `sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`
        3. `sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116`
    2. Enable the STRANDS repositories:
        1. Add the STRANDS public key to verify packages:
       `curl -s http://lcas.lincoln.ac.uk/repos/public.key | sudo apt-key add -`
        2. Add the STRANDS repository: `sudo apt-add-repository http://lcas.lincoln.ac.uk/ubuntu/main`
    3. update your index: `sudo apt-get update`
    4. install required packages: `sudo apt-get install libgmp-dev openjdk-8-jdk ros-kinetic-desktop-full ros-kinetic-strands-morse ros-kinetic-strands-ui ros-kinetic-strands-navigation  ros-kinetic-task-executor python-catkin-tools`
    5. initialise and update rosdep: `sudo rosdep init && rosdep update`
3. Try out the “MORSE” simulator (run all the following in your terminal): 
    1. configure ROS: `source /opt/ros/kinetic/setup.bash`
    2. launch the simulator: `roslaunch strands_morse tsc_fast_morse.launch`
    You should see the Morse simulator popping up with our robot platform being configured. 


4. Add the line `source /opt/ros/kinetic/setup.bash` to your `.bashrc` if you want ROS to be be loaded every time you open a terminal. You will be opening a few terminals, *so this is advisable*. Also, source it on the current terminal session:

```bash
source /opt/ros/kinetic/setup.bash
```

# Configuration

Next, create the local directory where you will do your work for this tutorial. This will contain a database and a [ROS catkin workspace](http://wiki.ros.org/catkin). The following steps will be basic steps we will assume you execute:

```bash
export WS_ROOT_DIR=~/rob_plan_ws
mkdir -p $WS_ROOT_DIR/src
```

Next clone this repository into the newly created src dir and create a directory within it for our database later on:

```bash
cd $WS_ROOT_DIR/src
git clone https://github.com/strands-project/planning_tutorial.git
mkdir planning_tutorial/db
```

Finally, if that was all successful, you should be able to build your workspace using [catkin tools](http://catkin-tools.readthedocs.io) (you can also use `catkin_make` if you prefer that approach).

```bash
cd $WS_ROOT_DIR
catkin init
catkin build
```

Assuming that was all successful you now need to source your workspace so you have access to the packages there. For this you can do 

```bash
source $WS_ROOT_DIR/devel/setup.bash
```

(e.g. `source ~/rob_plan_ws/devel/setup.bash` if you used the default from above). As with the other `setup.bash` file mentioned above, you need to source this file in every terminal you open, therefore it is *highly advisable* to add this command to the end of your `.bashrc` file so that it is done automatically. You can test if the file was sourced by running `roscd` (with no arguments). It everything was sourced correctly, this should take you to the directory `$WS_ROOT_DIR/devel/`.


# Tutorial Packages

For the tutorial itself you will be asked to write some simple Python scripts to control a robot in ROS via our planning and execution packages. You will be taught about these packages in the tutorial itself, but  we will not have time to cover the basics of Python. If you are not familiar with Python, it would certainly help to run through a quick Python tutorial (there are many online) to get comfortable with the language. That said, we will try our hardest to ensure that you can access as much of the tutorial material as possible without knowing Python. 


