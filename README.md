# Intelligent_Picking

(This was amongst the top 100 prototypes in Flipkart Grid 2.0 )

This problem statement tries to replicate the quintessential warehouse problem of picking in which the
participants are supposed to build their own robot hardware and software (collectively, a “Robot”) that is
capable of doing general tasks of picking items from a pick area and place them into a cell in th drop/stow
area. The problem statement requires a combined knowledge of object recognition, grasp planning, motion
planning and error recovery.

Details
  1. Objective :
      The robot needs to demonstrate following capabilities
      a. Ability to pick an object from the pick area
      b. Ability to move the object from the pick area to the drop area
      c. Ability to stow the removed object into a cell in a drop area.
  2. Pick Area/Drop Area:
      a. The pick area and drop area are two square areas (2m*2m)
      b. The drop area is further segmented into a grid (5rows x 5cols)
      c. The pick area will be 750-900mm higher than the drop area.
      d. The pick area and drop area should be identified by a colored tape (~2 inches) running across
      the boundary of the square
 
 Solution:
 Robot Technical & physical specifications:
 For Movement
->RDrive 70 (1 for each of the arms "A","B","C“ and for wheels)
->Relay to connect the motors to the High Voltage Power supply
->10Kg Metal Gear Servo Motor-Mg995(Small servo that can be connected to Raspberry Pi for the wrist movements)
-> 4 Wheels
-> Robotic servo drive(for each motor)
Physical Built
->High Density Polymer Arm and Palm Exteriors : Less weight (0.97g/𝑐𝑚3 density) compared to the metals and sturdy enough for our job
->Heavy metal base of diameter about 20 cm
Grip
->Silicon layer with a larger inflatable top (to ensure bending)
->Thin wires to wound around the fingers to make it firm
->Capacitive pressure sensor for each of the fingers to ensure that the item is gripped properly.
->Rubber tubes(at least 350 cm long) with plastic connectors (that connect to the finger)
->Air compressor to fill in air inside the fingers
Gears
->Worm gears (for connecting all the motors to the axles
->Torque enhancing gears for some parts like arm B and C ( To be put with the worm gears)
Electronics
->Laser (650nm 5mW 5V laser )
->Photodiode(It will detect the laser spot and then it'll make the arm move towards it.)
->Relay for Motor connections with high voltage supply
->HX710B Air Pressure sensor: to check that we don’t overfill the fingers
->Normal pressure sensor to check that we have actually gripped the object
->Camera module v2
->HCSR04 Ultrasonic Sensor Module Distance Measuring Sensor : To ensure that we are close enough to initiate gripping the object
->IR Receiver

 
❏	What all can the robot do?
❏	What all activities can it perform?
->Accurate object recognition
->Efficient gripping system to hold various kinds of objects
->Drops the object at a particular location as specified by the user
->For emergency situations:
=>Remote control
=>Kill switch(allows us to stop the robot if it starts showing any major faults)

❏	Are there any things that the robot can do above and beyond the
requirement?
❏	Are there any out of the box functionalities?
->The gripping system uses soft robotics which enables it to efficiently pick all sorts of objects.
->The robot can navigate through a space ,efficiently dodging obstacles and optimizing its path.

![image](https://user-images.githubusercontent.com/67795151/126879653-d3d1aafb-bb7e-436c-958d-c600104016bb.png)

What programming language will be used?

Python is used for the required Raspberry Pi programming(for YOLO and for all other modules to be built).
YOLO is implemented using Darkflow (Tensorflow and Darknet). YOLO sees the entire image during training and  test time so it implicitly encodes contextual information about classes as well as their appearance. YOLO trains  on full images and directly optimizes detection performance and learns generalizable representations of objects  in this manner.

What all software modules will be built?

Object detection module(includes image classification)
Laser following module
Gripping module
Capacitive pressure sensor module
Remote module(for input and navigation)
Sensor module for obstacle avoidance during navigation(extra feature)
Locomotion module(extra feature)

We'll fix an origin point from where the robot will start functioning.
The robot is maintains a database of all the possible items that it might have to pick.
User specifies what to pick using remote.
STEP 1 : Detects the object using YOLO in TensorFlow; an imaginary rectangle is drawn around the identified object.  STEP 2 : The arm adjusts itself so that the laser points to the center of the imaginary rectangle.
STEP 3 : Inverse kinematics allows all the arm lengths to adjust accordingly to give the most efficient movement.
STEP 4 : Once the hand is close enough the photodiodes will help the arm to move towards the laser point more precisely.  STEP 5 : Distance sensor senses that the object is close enough to initiate gripping.
STEP 6 : Initiates gripping : Air through the compressor fills in the inflatable fingers, air pressure sensor tells when to stop.  STEP 7 : Normal pressure sensor checks whether we have actually gripped the object.
STEP 8 : Arm 'A' lifts, Arm ’C' rotates all the way towards the drop area
STEP 9 : Camera module using TensorFlow detects the number on the grid where the item has to be delivered and makes an imaginary rectangle around it.  STEP10 : The arm adjusts itself so that the laser points to the center of the imaginary rectangle.
STEP11 : Inverse kinematics allows all the arm lengths to adjust accordingly to give the most efficient movement.
STEP12 : Once the hand is close enough the photodiodes will help the arm to move towards the laser point more precisely.  STEP13 : Distance sensor senses that the object is close enough to initiate gripping.
STEP14 : Relax grip and release item.  STEP15 : Return to origin.
///REPEAT//////

![image](https://user-images.githubusercontent.com/67795151/126880081-6efd20e5-737b-436f-8ac4-01336413a614.png)

Execution Plan
High level action items in terms of what will be the steps from the drawing board to the  actual prototype.

Assemble or 3D print parts(as mention in Physical specification) to make the frame(as in the CAD drawing).
Write algorithms and make the necessary software modules.
Train YOLO on a database according to the objects available.
Load the modules into the Raspberry Pi.
Check camera and laser modules.
Establish connections between
sensor and micro controller;  camera and micro controller;  laser and micro controller;  motors and micro controller;  wheels to motors.
Mount electronics to the frame.
Connect it to a power supply.
Check the kill switch.
Check the general working of the prototype.

![image](https://user-images.githubusercontent.com/67795151/126880262-b6d1604f-af03-4085-b7ff-40a04dd56097.png)
![image](https://user-images.githubusercontent.com/67795151/126880267-b550e6c7-9122-426d-90a5-ffc0c7238aa5.png)

The gripping system that we are using is inspired from soft robotics. We are
using inflatable rubber like material backed by some stiffer rubber sheet.

As the air fills into the empty rubber fingers it’ll force the backing rubber sheet to  bend and it will be very precise in gripping the contours of the object because of the  flexibility that rubber provides.

Work Envelope and Payload calculation
Reach of the robot across the pickup/drop area
The robotic arm is placed on one corner of the pick area (closer to the drop area) and  reaches both the farther corners of the pick and drop area.

Degrees of Freedom
Our arm has 3 degrees of freedom


