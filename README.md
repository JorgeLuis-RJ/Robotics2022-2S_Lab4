# Robotics2022-2S_Lab4
National University of Colombia<br/>
Bogota Campus

Robotics<br/>
2016770 - 3

Jorge Luis Reina Jara<br/>
25481216

## Laboratory 4 - Forward Kinematics, Phantom X & ROS

![This is an image](/assets/PhantomXPincher_Presentation.png)

### Introduction
This repository tries to show the development of the Fourth Laboratory in the Robotics course of the National University of Colombia. It turned around the control and execution of some code to manipulate, at a joint level, the configuration of the PhantomX Pincher Robot, a very useful and widespread learning tool in the robotics world. The main goal was to stablish an effective connection to some of the ROS topics and services related to the PhantomX manipulator to publish the desire joint angle values and also, to monitor them by the exact subscription.

### Methods
#### Forward Kinematics

![This is an image](/assets/Workspace_PhantomXPincher.png)

The first step in the way of getting to know the PhantomX manipulatior is to comprehend the nature of all joints in the robot. One of the prime caracteristics in the PhantomX robot is its 5 degrees of freedom (DOF) represented in five rotational joints that controls all the movements. There are four of them that are specifically engaged of positioning the robot's end effector in a certain pose, i.e. joints 1 to 4, and the last one, joint 5, is the one which opens and closes the gripper attached to the last link. The following picture illustrates the frames of reference chosen, based on the Denavit and Hartenberg conventions (D&H), for describing conveniently all the parameters of the robot.

![This is an image](/assets/PhantomX_FR.png)

And according to those frames of reference, here are each of link's D&H parameters.

![This is an image](/assets/PhantomX_D&H.png)

In the last table, and based on the first picture of this section, the workspace of the PhantomX Pincher robot, the generic parameters d1, a2, a3 and a4 are 42, 105, 105 and 110 mm, respectively.

#### Movement Through ROS

In order to generate the first commands that pushes the PhantomX arm robot into a desire configuration, it is mandatory to initialize the ROS master module and to interact with the precise topics, essentially, the _joint_trajectory_ one. For doing so, it was used the _basic.yaml_ file for naming correctly all joint names; the _one_controller.launch_ file for specifying the right USB port employed to comunicate from the computer to the manipulator hardware; and finally, the _lab4_jointPub.py_ script file to declare all desire joint angles and sleep times for asking the PhantomX to move. The next piece of code is the core of that last Python language script.

```
    while not rospy.is_shutdown():
        state = JointTrajectory()
        state.header.stamp = rospy.Time.now()
        state.joint_names = ["joint_1", "joint_2", "joint_3", "joint_4", "joint_5"]
        point = JointTrajectoryPoint()
        point.positions = [0, 0, 1.57, 0, 0]    
        point.time_from_start = rospy.Duration(0.5)
        state.points.append(point)
        pub.publish(state)
        print('published command')
        rospy.sleep(1.5)
        
        state = JointTrajectory()
        state.header.stamp = rospy.Time.now()
        state.joint_names = ["joint_1", "joint_2", "joint_3", "joint_4", "joint_5"]
        point = JointTrajectoryPoint()
        point.positions = [0.52, -0.52, 0.52, -0.52, 0]
        point.time_from_start = rospy.Duration(0.5)
        state.points.append(point)
        pub.publish(state)
        print('published command')
        rospy.sleep(1.5)
```

As it may be noted, the same lines of code had been copied (five times actually) in order to get a paused movement one joint at a time, going from the _home_ comfiguration at [0, 0, 1.57, 0, 0] to the arbitrary one at [30°, -30°, 30°, -30°, 0] (although, in the code was written in radians), by changing one by one the joint angle values.

#### Creation of the Peter Corke Toolbox Model



### Results

Implementing the procedure carried out through ROS, it was obtained one set of trajectories, shown in the next videos.

[![This is a video](https://img.youtube.com/vi/dXPQGdM8PHg/hqdefault.jpg)](https://youtu.be/dXPQGdM8PHg)
