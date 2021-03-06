# autonomous-EV-charger

## Background
With the climate crisis that's happening right now, it's now more important than ever to take action and start adopting sustainable practices before we cause irreversable damage to our planet. A popular trend is the shift towards electric vehicles, and in an effort to make these vehicles more attractive, our team was tasked with creating an autonomous charger robot that would automatically find, and plug the charging line into the car after the car is parked. Our team is comprised of Anton Liu, Haoran Jayce Wang, and Kelvin Cui, and this project was created for our ESC204 (Praxis 3) class from the University of Toronto Engineering Science program.

<img src="https://i.ibb.co/129g9J2/autonomous.jpg" alt="autonomous" border="0">

## Objective
Our objective was to create a robot that could locate the charging port of an electric vehicle, and plug in a standard charging plug. We wanted the robot to take up as little space as possible, be consistent and reliable, all while constraining ourselves to a budget of $300 CAD. 
<img src="https://i.ibb.co/TKp0mkW/Main.png" alt="Main" border="0">

## How it works
The robot is controlled by a raspberry pi, using computer vision to determine the location of the port. Two IR distance sensors on each side of the bot determine the yaw of the bot with respect to the side of the car, and allow the bot to align itself perpendicularly. Stepper motors arranged in a square at the bottom of the bot allow the robot to rotate, and move in a straight line until it's directly in front of the charging port. Another stepper motor attached to a lead screw control a scissor lift mechanism that lifts the charging handle to the same height as the port. Finally, the robot moves forward to plug the handle in, where a limit switch lets the bot know when the handle is fully plugged in.
<img src="https://i.ibb.co/3hRGWwT/Plug-Quarter-View.png" alt="Plug-Quarter-View" border="0">

## Design Process
In order to design this robot, we started by defining our stakeholders, our objectives, and our metrics to measure these objectives. Using this model, we started multiple rounds of design, going through many different alternatives until we finally arrived at our solution. If you're interested, you can see the detailed request for proposal here : 

**[Request for Proposal](https://github.com/ntnox/autonomous-EV-charger/blob/master/Request%20for%20Proposal.pdf)**

### Hardware Choice
Given our time and budget constraints, most of our electronics and sensors are simple, off the shelf components. We chose a raspberry pi as it allowed us to do complex computer vision analysis to find the charging port, with a raspberry pi camera. We chose IR distance sensors over ultrasonic sensors as they were more consistent, and delivered more accurate results in our range. For motors, we chose stepper motors, despite their increased complexity (more power draw and stepper motor drivers), as it would allow for finer control when compared to simple DC motors. Finally, we decided to go with a fixed power supply rather than a battery pack simply because it better fit our budgetary constraints. Finally, the omni-wheels were chosen to allow the rover to move freely, and rotate on the spot.

### Physical Design
Next came our design. We decided to model the entirety of our design, and take advantage of the rapid prototyping tools we had in our disposal. With a model, we were able to either lazer cut or 3D print all of our components.

<img src="https://i.ibb.co/cTCK34R/Mechanical-CAD.png" alt="Mechanical-CAD" border="0">

While creating the model, we needed to make sure that from a strength perspective, our design would be feasible. As a result, we did FEA anaylsis of all of our structural parts to ensure that the parts wouldn't fail.

<img src="https://i.ibb.co/1qKz7cp/Untitled-drawing-25.png" alt="CAD FEA" border="0">

### Electronic Design
For electrical design, we wanted to make sure that it was relaible, and we wanted it to be as easily repairable as possible should anything go wrong. As a result, our circuitry design is based on solderable breadboards for secure connections, with color coded wiring to easily distinguish the ciruits. In particular, since we had two power rails (5V and 24V), we used red wiring for all 5V, white for all 24V, and black for all ground. The main circuit was connected to the sensors, motors, and raspberry pi via JST headers, and could be easily taken out of the robot for repairs.

<img src="https://i.ibb.co/61pWSQ2/Electrical-Circuitry.jpg" alt="Electrical-Circuitry" border="0">

### Software Design
- All the software is written in Python and executed by the onboard Raspberry Pi Zero. The goal of the software was to be able to autonomously detect the charging port and be able to align and navigate the charger into the charging port. The last developments made in the area of port detection used OpenCV to process frames from a live video feed to identify set markers on the charging port prototype. 
- Various methods were prototyped to experiment with the best approach to reliably identify and localize the robot with respect to the port through OpenCV that can be read about in the Programming folder, but the finalized approach was to use a red rectangle marked ontop of the port as the localization identifier. This method is the most reliable as our approach was to create a mask that only allowed pixels that match the redness of the marker and ultimately identify the contour of the rectangle in that mask. Since we knew exactly where our camera was placed on the robot, with some calibration and testing, we can then use the location of the rectuangular contour on the frame to localize the robot. To maximize the frames per second the robot is able to process using OpenCV, the task of generating the video stream from the camera and the processing of frames were completed on multiple threads or paralleled computations. This method was able to significantly improve our processed video stream from less than 10 fps to more than 15 fps.

- To control the movement of the robot, we utilized the integrated GPIO library in Python to output corresponding digital signals to control stepper motors as required
- Wireless communication for remote control of the robot from a laptop when required was built using the socket library in Python to create a server for sending desired commands to the rasberry pi and placing both the control laptop and the raspberry pi to be on one hotspot network so that they could communicate over the shared wifi. 

### Project Management
- Trello was used to manage this project, and this includes aspects such as due dates, person assigned, descriptions, categorization, etc. It gives us a clear view of the status of the project, and keeps us on track to complete it. We have also included a "Risks" section to identify potential failure modes of the design, and planned solutions for them. With trello, we were able to keep the project organized and efficient.

<img src="Pictures/Trello overview.JPG">

- We also used google sheets to create a Bill of Materials (BOM), which contains name, price, purchaser, and original URL of all the parts that we have used. As we are on a tight budget limit, the BOM helped us to identify priorities and where to cut costs. 

<img src="Pictures/BOM.PNG">

## Lessons Learned
If we were to do this project again, there are definately some things that we would do differently. The full raspberry pi was overkill, and a raspberry pi zero has enough computational power to do the calculations, at a fraction of the cost. The scissor lift design was a pain, since the motor did not linearly move the plug up and down. A vertical "elevator" design would have worked better, using the exact same hardware, with the lead screw directly driving the plug platform vertically.
