# NAO setup and deployment
Before you can deploy the NaoTH Code to the robot needs to be set up in a special way. This is a one time setup which 
replaces the operating system that comes with the robot and additionally sets up some libraries we need on the robot. 

## Setup Routine for V5 Robots
First we need to set up the operating system with the image provided by softbank robotics. You can download it from 
[here](https://www2.informatik.hu-berlin.de/~naoth/ressources/Softbank/nao-2.8.5.11_ROBOCUP_ONLY_with_root.opn)

This image must be put onto a usb drive. For this you can use the 
[Nao Flasher](http://doc.aldebaran.com/2-8/software/naoflasher/naoflasher.html?highlight=naoflasher) tool provided by softbank.
The Nao Flasher tool expects the usb drive to not have any filesystem. In order to flash the robot, make sure the robot
is turned off then insert the usb stick and press the chest button until the blue chest LED's blink. 
The flashing will take a couple of minutes.

The robot should be in the monkey pose during the flash process. After this is finished you can set the robot upright.
If it is the first time the robot is flashed with the image, it will then
stand up and calibrate its joints. During the calibration the robot must stand on an even surface.

![monkey_pose](../../img/naoth_setup/robot_poses.png)  
Left: monkey pose. Right: sit pose

The next step is to initialize the robot with the required libs. This can be done with NaoSCP and is described
[here](../../naoth_tools/naoscp.md)

After that the compiled robot code can be deployed with NaoSCP as well.


## Calibrating Joints 

The default software of the robot has an integrated automatic calibration routine for the alignment of the joints.
This routine is usually executed only at the first start of a new robot.
During the RoboCup games it can happen (and it does happen) that the joints lose their alignment, 
which might negatively affect all motions, e.g., the walk become a bit more unstable. 
The calibration values are stored internally and are preserved even when the OS is replaced.
The following describes how the automatic calibration routine can be triggered when needed.

### Trigger the Automatic Calibration Routine

1. prepare a USB stick with NaoOS Image (as described above) with factory reset.

2. flash the robot:
    1. turn off the robot
    2. insert the USB stick with the system
    3. press the chest-button **until it turns blue** (this takes about 3s), then release the button
    4. (the button should blink blue very fast)
    5. wait until the flashing is done (can take 10 min) (robot in the monkey pose)
    6. wait until the robor says either 
        - **(A)** "Good morning. Please put me in an open space on the floor and touch my head or my bumper, so I can wake up correctly."
        - **(B)** "I'm not safe like this ... etc."

3. if the robot says **(A)**, then the robot is ready to calibrate:
    1. put the robot on the solid floor (not carper, not table) with enough space around it (50cm) in a crouch position.
    2. touch robot's head or a foot bumper 
    3. robot will perform a "waking up" procedure
    4. wait until the robot sits down and says "Thanks, now I feel great."

4. if the robot says **(B)**, then we need to manually delete old calibration files to trigger the calibration routine:
    1. connect a LAN cable to the router and wait some seconds
    2. press robot's chest button
    3. the root will say it's ip-address
    4. connect to the robot with `ssh`
    5. create a new directory (if it doesn't exist)
       ```sh
       sudo mkdir /media/internal/diagnostic.bak
       ```
    6. move old diagnostic files into the new directory
       ```sh
       sudo mv /media/internal/diagnostic_* /media/internal/diagnostic.bak
       ```
    7. reboot the robot
      ```sh
      sudo reboot
      ```
    8. after the startup the robot should say **(A)**
    9. go to 3.
    
5. now the robot is calibrated
    1. turn off the robot
    2. proceed with the setup of the Ubuntu system

### Reference

* <https://www.robotlab.com/support/nao-will-not-stand-up>



## New Setup Routine (with custom image)
For the NAO V6 robots we will use a custom ubuntu based image. Each robot has to be set up with this image once. For creating 
the image see the NaoImage repository in gitlab. (Not public). The instructions for building and deploying the image can
be found there.




---
## Deploy Code to robot
TODO: explain network stick  
TODO: explain deploy stick  
TODO: explain deploy routines?





