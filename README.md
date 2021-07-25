# Intelligent_Picking

(This was amongst the top 100 prototypes in Flipkart Grid 2.0 )

This problem statement tries to replicate the quintessential warehouse problem of picking in which the
participants are supposed to build their own robot hardware and software (collectively, a “Robot”) that is
capable of doing general tasks of picking items from a pick area and place them into a cell in th drop/stow
area. The problem statement requires a combined knowledge of object recognition, grasp planning, motion
planning and error recovery.

<p align="center">
<img src="https://user-images.githubusercontent.com/67795151/126879653-d3d1aafb-bb7e-436c-958d-c600104016bb.png">
</p>

## [Simulation and Cad drawings↗](https://drive.google.com/drive/folders/1JTeQxToqlkp2bXdBVYcgdZcWhLkdDqt7?usp=sharing)

## Details
  1. The robot demonstrates following capabilities:
      1. Ability to pick an object from the pick area
      2. Ability to move the object from the pick area to the drop area
      3. Ability to stow the removed object into a cell in a drop area.
      4. Accurate object recognition
      5. Efficient soft robotic gripping system to hold various kinds of objects
      6. Drops the object at a particular location as specified by the user
      7. Remote controlled
      8. Has a kill switch (allows us to stop the robot if it starts showing any major faults)
      9. The robot can navigate through a space ,efficiently dodging obstacles and optimizing its path.
      
  2. Pick Area/Drop Area:
      1. The pick area and drop area are two square areas (2m*2m)
      2. The drop area is further segmented into a grid (5rows x 5cols)
      3. The pick area will be 750-900mm higher than the drop area.
      4. The pick area and drop area should be identified by a colored tape (~2 inches) running across
      the boundary of the square
 
## Technical & physical specifications of the Robot:
 
 ### For Movement:
- RDrive 70 (1 for each of the arms "A","B","C" and for wheels)(Refer to diagram below)
- Relay to connect the motors to the High Voltage Power supply
- 10Kg Metal Gear Servo Motor-Mg995(Small servo that can be connected to Raspberry Pi for the wrist movements)
-  4 Wheels
-  Robotic servo drive(for each motor)
### Physical Built:
- High Density Polymer Arm and Palm Exteriors : Less weight (0.97g/𝑐𝑚3 density) compared to the metals and sturdy enough for our job
- Heavy metal base of diameter about 20 cm 
### Grip
- Silicon layer with a larger inflatable top (to ensure bending)
- Thin wires to wound around the fingers to make it firm
- Capacitive pressure sensor for each of the fingers to ensure that the item is gripped properly.
- Rubber tubes(at least 350 cm long) with plastic connectors (that connect to the finger)
- Air compressor to fill in air inside the fingers 
### Gears
- Worm gears (for connecting all the motors to the axles
- Torque enhancing gears for some parts like arm B and C ( To be put with the worm gears)
### Electronics
- Laser (650nm 5mW 5V laser )
- Photodiode(It will detect the laser spot and then it'll make the arm move towards it.)
- Relay for Motor connections with high voltage supply
- HX710B Air Pressure sensor: to check that we don’t overfill the fingers
- Normal pressure sensor to check that we have actually gripped the object
- Camera module v2
- HCSR04 Ultrasonic Sensor Module Distance Measuring Sensor : To ensure that we are close enough to initiate gripping the object
- IR Receiver
<p align="center">
<img src="https://user-images.githubusercontent.com/67795151/126880081-6efd20e5-737b-436f-8ac4-01336413a614.png" width="700px">
</p>

## Work Envelope and Payload calculation
Reach of the robot across the pickup/drop area
The robotic arm is placed on one corner of the pick area (closer to the drop area) and  reaches both the farther corners of the pick and drop area.

Degrees of Freedom
Our arm has 3 degrees of freedom ->
<img align="center" src="https://user-images.githubusercontent.com/67795151/126888375-e8a14f14-6ef4-4872-be37-220c99689209.png">
<p align="center">
<img src="https://user-images.githubusercontent.com/67795151/126888495-0737e310-a08e-48cc-91ee-e488b80cc5a0.png" width ="700px"> <img src="https://user-images.githubusercontent.com/67795151/126888501-2e4d264b-06c0-4343-a6ef-d6d5e6d4cdb5.png" width ="700px">
<img src="https://user-images.githubusercontent.com/67795151/126888602-e6cdd098-4f7e-4140-ae05-728ab2b6273c.png" width ="700px"><br/>
Since both A and B have to be equal,  length of A = length of B = 152cms

</p>

## Gripping System
<p align = "center">
<img src="https://user-images.githubusercontent.com/67795151/126880262-b6d1604f-af03-4085-b7ff-40a04dd56097.png"> <img src ="https://user-images.githubusercontent.com/67795151/126880267-b550e6c7-9122-426d-90a5-ffc0c7238aa5.png">
</p>
The gripping system that we are using is inspired from soft robotics. We are using inflatable rubber like material backed by some stiffer rubber sheet.
As the air fills into the empty rubber fingers it’ll force the backing rubber sheet to  bend and it will be very precise in gripping the contours of the object because of the  flexibility that rubber provides.
After detecting the object, a laser will be pointed at the  object location, which further is detected by photodiode. This  directs the arm from its initial position to the object position.
The object is picked up and moved to the drop position  along a pre defined path. The grip is released to place the  object at drop point. Now grip planning should also include  which object to pick up at any given time for that they should  provide us info on what objects are there for pick up

## Programming

Python is used for the required Raspberry Pi programming(for YOLO and for all other modules to be built).
YOLO is implemented using Darkflow (Tensorflow and Darknet). YOLO sees the entire image during training and  test time so it implicitly encodes contextual information about classes as well as their appearance. YOLO trains  on full images and directly optimizes detection performance and learns generalizable representations of objects  in this manner.
<p align="center">
<img src="https://user-images.githubusercontent.com/67795151/126888645-786c80e9-9b81-4d6f-9403-6f569320f871.png">
</p>

### Software modules

- Object detection module(includes image classification)
- Laser following module
- Gripping module
- Capacitive pressure sensor module
- Remote module(for input and navigation)
- Sensor module for obstacle avoidance during navigation
- Locomotion module

## Working

We'll fix an origin point from where the robot will start functioning.
The robot is maintains a database of all the possible items that it might have to pick.
User specifies what to pick using remote.
1. Detects the object using YOLO in TensorFlow; an imaginary rectangle is drawn around the identified object.
2. The arm adjusts itself so that the laser points to the center of the imaginary rectangle.
3. Inverse kinematics allows all the arm lengths to adjust accordingly to give the most efficient movement.
4. Once the hand is close enough the photodiodes will help the arm to move towards the laser point more precisely.
5. Distance sensor senses that the object is close enough to initiate gripping.
6. Initiates gripping : Air through the compressor fills in the inflatable fingers, air pressure sensor tells when to stop.
7. Normal pressure sensor checks whether we have actually gripped the object.
8. Arm 'A' lifts, Arm 'C' rotates all the way towards the drop area.(Refer to above diagram)
9. Camera module using TensorFlow detects the number on the grid where the item has to be delivered and makes an imaginary rectangle around it.
10. The arm adjusts itself so that the laser points to the center of the imaginary rectangle.
11. Inverse kinematics allows all the arm lengths to adjust accordingly to give the most efficient movement.
12. Once the hand is close enough the photodiodes will help the arm to move towards the laser point more precisely.
13. Distance sensor senses that the object is close enough to initiate gripping.
14. Relax grip and release item.
15. Return to origin.<br/>
//REPEAT//

## Execution Plan

1. Assemble or 3D print parts(as mention in Physical specification) to make the frame(as in the CAD drawing).
2. Write algorithms and make the necessary software modules.
3. Train YOLO on a database according to the objects available.
4. Load the modules into the Raspberry Pi.
5. Check camera and laser modules.
6. Establish connections between sensor and micro controller;  camera and micro controller;  laser and micro controller;  motors and micro controller;  wheels to motors.
7. Mount electronics to the frame.
8. Connect it to a power supply.
9. Check the kill switch.
10. Test the prototype.


