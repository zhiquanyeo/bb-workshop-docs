# Workshop Robot Runbook/Instructions

## Getting Started

### Overview
Each workshop robot is controlled by a Raspberry Pi and robot control board. This is a standalone system and allows for users to connect motors, sensors and outputs in order to build a working, programmable robot.

Each robot broadcasts a private wireless network that allows users to connect to the robot and interact with it.

The system is designed to be powered off 4 AA batteries.

### Basic Wiring
The bare minimum configuration for the robot is the Raspberry Pi + Controller board, and a 4x AA battery pack. The battery pack provides power for the motors and the electronics. The battery pack should be connected to the screw terminals marked PWR (red wire) and GND (black wire).

Up to 2 motors can be connected to the robot, and these can be wired up to the ports marked M1 and M2.

### Power
The robot control board has a small slide switch next to the micro USB port. Slide the switch AWAY from the USB port to turn the robot on. A blue LED will come on indicating that the board is powered correctly.

### Connecting to the robot
Identify the robot you are working with. There should be a label on the board that indicates the robot's name. This is usually the name of a cheese. Just because.

Once the robot has been identified and powered on, wait a minute and see if you can find a wifi network with the name of the robot, appended with "-bot". For example, if you are working with the robot named "pecorino" (mmm cheese), you should be able to see a wifi network called "pecorino-bot".

Select that network, and when prompted for a password, enter cheesebot. If all goes well, you should be connected to the robot. Ignore any warnings about limited connectivity.

### Launching the IDE
Using the Chrome web browser, visit http://192.168.42.1:3000. A web page should then appear, with a set of buttons on top, a text editor on the left and a series of tabs on the right. This is the editor you will use to write code for the robot.

### Using the IDE
Most of the time will be spent on the left pane, which is the code editor view. The robot is programmed in Java, in a similar style to how regular FRC robots are programmed, using wpilibJ. There is only a single file, `TestRobot.java`, that exposes a single class TestRobot. All edits will be made in this file.

On the right are 4 tabs. The _Output_ tab provides messages from the system when compiling your code. The _Console_ tab provides messages from the robot when it is running. Any calls to `System.out.println` will show up here. For both of these tabs, you can press the Clear button to clear out existing messages.

The _Reference_ tab provides useful method calls for writing robot programs. The _Snippets_ tab contains a set of useful code snippets that can be copied into the text editor, as well as usage instructions.

On the bottom of the screen are 3 groups of text. The first group, labeled client ID, is an indicator of what the server knows you as. Some of the names are hilarious. The second group will be green and have the word "Connected" in it if you are connected to the robot, and you are first in line for running your code. If it is red and says "Disconnected", the robot is offline. If it says "You are #X in
the queue", it means that you'll have to wait your turn before you can run your code on the robot. The final group indicates if a gamepad or joystick has been connected. The system currently only supports 1 joystick/gamepad.

Finally, at the top of the screen is a toolbar. The "Compile and Run" button will attempt to build a robot application using the code you wrote, and start it. If there are any compilation errors, or errors while running the program, they will appear in the Output tab. Once an application is running, the "Stop App" button can be used to terminate it. When you are done playing with the robot, press "Give up position" to let the next person in line have a go. You will automatically be put at the back of the line.

There are also indicators and selectors for setting the different modes on the robots, and these will be covered in the next section.

**IMPORTANT NOTE** There is no auto-save functionality (and ctrl+S doesn't do what you think it does). COPY code OFTEN to a scratch pad to ensure you don't lose your work! On the plus side, even if the server crashes, as long as you don't refresh the page (this will reset the code to the default template), your code will still be in the editor.

### Running your program
Once you have written your program, press "Compile and Run". Whenever it is done building, the _Console_ tab should automatically select itself, and you should see some text there that indicate the status of the robot program.

The robot will always start in disabled mode, and the current mode is visible in the toolbar. Pressing any of the mode buttons in the toolbar (disable, teleop, autonomous) will automatically switch the robot to that mode, and execute the appropriate code block in your program.

Any output from the robot (via `System.out.println`) will automatically appear in the Console tab. When you are done testing your program, press the "Stop App" button to kill the program.

### Using a joystick/gamepad
If you want to use a joystick, make sure the IDE is in focus before you plug it in. Once plugged in, press a button or move a stick around to make sure Chrome detects it. If all goes well, the Gamepad Conencted message should appear on the bottom, in green.

### Some guidance for new mentors
The robot code is based off wpilib, and a good reference is
https://wpilib.screenstepslive.com/s/currentCS/m/java As a note, we use the `TimedRobot` template.

The robot code is structured as a set of methods, that get run whenever a robot is in a specified state. For example, the `public void autonomousInit()` function gets run whenever the robot ENTERS the autonomous state (from some other state), and the `public void autonomousPeriodic()` function gets run, as its name suggests, periodically while the robot is in autonomous mode.

As an example:
```java
public void autonomousInit() {
    System.out.println("I'm an autonomouse!");
}

public void autonomousPeriodic() {
    System.out.println("Squeak");
}
```

will print out:

```
I'm an autonomous!
Squeak
Squeak
Squeak
 :
 :
 :
```
If the robot is switched to a different mode (e.g. teleop, disabled), then the appropriate `*Init()` and `*Periodic()` methods for those modes will get called.

Robot code tends to resemble a loop that involves sensing, acting, sensing, acting, etc. This is the _sense-act_ loop. Since the periodic functions essentially perform the looping for us, a useful sense-act loop in teleop might look like this:

- Sense: Read Joystick Inputs
- Act: Power the motors appropriately
- Repeat

So, and example of this in code is:

```java
public void teleopPeriodic() {
    // SENSE
    double moveVal = gamepad.getRawAxis(1); // Left stick on the gamepad, y-axis
    double turnVal = gamepad.getRawAxis(2); // Right stick, x-axis

    // ACT
    drivetrain.arcadeDrive(moveVal, turnVal);
}
```

Similarly, in autonomous mode, we could do:

```java
public void autonomousPeriodic() {
    // SENSE
    // Read in some data
    double move = getMoveValueFromSuperSecretAlgorithm();
    double turn = getTurnValueFromSuperSecretAlgorithm();

    // ACT
    drivetrain.arcadeDrive(move, turn);
}
```

In summary, for new mentors, read the docs on screenstepslive :) Or, reach out to Joe or ZQ!

## Potential Problems and Solutions
**Robot does not power on** Ensure that the power cables from the battery pack are installed correctly. Red to PWR and black to GND

**No Wifi network found** Ensure that the SD card is firmly in the Raspberry Pi. Power cycle the robot. If this still does not fix the issue, swap out for a new raspberry pi, and mark this one as "damaged".

**Cannot connect to wifi network** Ensure you have the correct password (cheesebot). If that fails, power cycle both robot and laptop. Try again. If it still fails, swap out for a new raspberry pi.

**IDE does not load** Ensure you are hitting http://192.168.42.1:3000

**Status bar suddenly says "Server Disconnected"** The server application has crashed. It should revive itself after 10 seconds. If after waiting 10-15 seconds, you still do not get a "Connected" or "#X in line" message in the status bar, save your work (select all, copy, paste into notepad), power cycle the robot, and close the tab with the editor. When the robot starts up again, paste your saved
work back in.

**Gamepad not connected** Try disconnecting and reconnecting the joystick, followed by pressing a button. If that does not fix it, there could be a problem with your gamepad. Try another one. If that still does not work, try restarting your computer. This also seems to be an issue on Chrome on Windows computers... Additionally, ensure that the selector switch on the back of the joystick is set to D.

**Console spew about joystick axis not found** Stop the running application. Copy and paste the code somewhere safe. Refresh the page, and paste the code back into the editor. Compile and Run again. It should fix itself.

**Robot not moving at all** Check if your code actually manipulates the drivetrain object. If it does, and nothing is happening, check that your motor connections are correct, and that you are writing to ports 0 and 1. Also try swapping out the batteries for fresh ones.

**Robot turning the wrong direction** Figure out which motor is flipped, and either swap the wiring for that motor port, or uncomment the line in robot code that flips the appropriate motor.
