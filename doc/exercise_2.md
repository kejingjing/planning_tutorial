# Exercise 2

In this exercise you will use our MDP/LTL planning framework to get the robot to perform 3D mapping sweeps of a simple. This example connects many of the different elements of our robot system, and may seem complex at first. If something is unclear, or you want more information, please just ask.



# Background

You must first run some basic elements from the STRANDS system. You should ideally run each of these in a separate terminal where you have sourced both the ROS and your local workspace `setup.bash` files, as described in [tutorial_prep.md](./tutorial_prep.md). 

## MongoDB Store

(If you still have the database running from [Exercise 1](./exercise_1.md) you can skip this step)

First, check the `db` directory exists (which you should've created following [tutorial_prep.md](./tutorial_prep.md)). The following should not list any files or report an error:

```bash 
ls `rospack find planning_tutorial`/db
```

If that is ok, then launch [mongodb_store](http://wiki.ros.org/mongodb_store) using that directory as its data path:

```bash
roslaunch mongodb_store mongodb_store.launch db_path:=`rospack find planning_tutorial`/db
```

## MORSE Simulation

In another terminal, launch our object simulation taken from the [ALOOF Project](http://www.dis.uniroma1.it/~aloof/). 

```bash
roslaunch strands_morse aloof_morse.launch 
```

If you press the 'h' key in MORSE you can see a list of available keyboard commands.

## 2D and Topological Navigation

We have predefined a simple topological map for you to use in this tutorial. The first time (and only the first time!) you want to use topological navigation in the ALOOF simulation, you must add this map to the mongodb store. Do it with the following command:

```bash
rosrun topological_utils load_yaml_map.py `rospack find strands_morse`/aloof/maps/aloof_top_map.yaml
```

You can check the result of this with the following command which should print a description of the topological map `aloof`.

```
rosrun topological_utils list_maps 
```

If this was successful you can launch the 2D (amcl and move_base) and topological localisation and navigation for your simulated robot. Note that in this configuration, the statistics for edge transitions in the topological map will be learnt from experience (as opposed to manually specified as in [Exercise 1](./exercise_1.md)).

```bash
roslaunch strands_morse aloof_navigation.launch
```

To see everything running, launch the ROS visualisation tool `rviz` with the provided config file:

```bash
rviz -d `rospack find planning_tutorial`/plan_tut.rviz
```

You'll find that the robot is not localised at the correct place compared to the simulation. Therefore use the `2D Pose Estimate` button and click (then hold and rotate to set angle) on the correct part of the map.

If you click on a green arrow in a topological node, the robot should start working its way there. Feel free to add whatever extra visualisation parts you want to this (or ask us what the various bits are if you're new to robotics).


## MDP Planning

Next launch the MDP-based task executive system in (yet another!) new terminal:

```bash
roslaunch mdp_plan_exec mdp_plan_exec_extended.launch
```

## 3D Mapping Nodes

Our 3D mapping framework makes use of an approach called *meta-rooms* which builds 3D maps at waypoints in the environment. Before you run this for the first time you need to create somewhere for the system to store maps. Do this with the following command:

```bash
mkdir ~/.semanticMap
```

If this was successful, you can launch the meta-room nodes with the following command:

```bash
roslaunch planning_tutorial meta_rooms.launch
```

# Exercise 2a

In [Exercise 1](./exercise_1.md) you exploited the fact that the execution framework automatically creates an MDP for navigation across the topological map. In this exercise we will extend this MDP with additional actions which connect ROS [actionlib servers](http://wiki.ros.org/actionlib) to actions in the MDP. 


