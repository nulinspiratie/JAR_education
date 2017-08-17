# Controlling the Pixhawk from a laptop via the Raspberry Pi

Information based on link: http://ardupilot.org/dev/docs/raspberry-pi-via-mavlink.html

This guide explains the setup needed to control the PixHawk from the laptop via the Raspberry Pi.
Control of the PixHawk from the laptop is achieved as follows: The Raspberry Pi is connected to the Pixhawk's TELEM2 port (see http://ardupilot.org/dev/docs/raspberry-pi-via-mavlink.html].
The Raspberry Pi hosts a Jupyter Notebook server, which can be accessed from a laptop on the same WiFi network.
A Jupyter Notebook is an interactive programming environment that allows easy evaluating of code sections.

This guide assumes that the Raspberry Pi is installed and setup correctly (see Raspberry pi installation guide).

## Setting up the Pixhawk
The Pixhawk should be set up using ArduPilot's Mission Planner, which should be installed on a computer.
Open Mission Planner, connect the PixHawk's micro-USB port to a computer USB port, and follow the setup wizard.
This wizard will setup several things of the Pixhawk, including:
- the accelerometer, 
- the compasses (both internal and the external GPS compass), 
- the RC receiver/transmitter (can be skipped if not used)
- Updating the firmware if necessary
- A geofence can be setup, which will stop the drone from flying too far away. This has not yet been tested, and it's not sure if it works with the Python dronekit.

### Flying Pixhawk via RC
After the setup is complete, the drone can be controlled via RC.
Be sure to have the drone outside, and wait a while for the GPS to localize the drone.
Before arming the drone, the switch should be pressed for at least 1 second. Its red flashing light should change to a continuous red light.
After this, the drone can be armed by moving the throttle button of the RC to the bottom right and hold it for a few seconds.
The drone can only be armed if the main Pixhawk LED is flashing a green light.
If it is not, then this means that one of the arming checks is unsuccessful. You can see which one this is by going to the main screen on Mission Planner and look at the red letters on the left screen, or look at the log at the bottom left.

### Overriding the arming checks
You can override many of the arming checks.
This can be useful if you for instance do not want to connect an RC receiver to the Pixhawk.
This can be done by going to config in Mission Planner, click on parameters, and then in arming checks, deselect `all`.
It is adviced to only deselect the arming checks you want to skip, and keep the remaining ones.

## Verify connectivity between Raspberry Pi and Pixhawk
After the Pixhawk has been set up, and connected to the Raspberry Pi (see above), the next step is to check if you can communicate between the Pixhawk and the Raspberry Pi.
Note that this step is only to verify the connectivity, to actually control the pixhawk, we will use a different approach.
These steps need to be performed on the Raspberry Pi.
Either connect it to a display using an HDMI cable, or use VNC viewer on a laptop (see Raspberry Pi installation guide).

- Open terminal
- Type `sudo mavproxy.py`

If all goes well, you should see quite some output on the screen, including `Received {number} parameters`.
This will signify that it has succesfully retrieved parameters from the Pixhawk.
Make sure you see a line mentioning the serial port, it should be something like `/dev/ttyS0`. Remember this port.

## Connect laptop to Pixhawk via Raspberry Pi.
Finally, the laptop can be used to control the Pixhawk via the Raspberry Pi.
The idea is that the Raspberry Pi is an intermediate device that is used to send signals back and forth between laptop and Pixhawk.
In particular, the Raspberry Pi should be connected to the same WiFi as the laptop, preferrably on a WiFi hotspot created by the laptop (see Raspberry Pi installation guide).
You should be able to control the Raspberry Pi via VNC viewer.
Make sure you know the ip address of the Raspberry Pi (can be found by typing `hostname -I` on the Raspberry terminal).
Next, open a browser on the laptop, and navigate to `{ip}:8888`, where `{ip}` is replaced by the Raspberry ip address.
A Jupyter notebook should now be opened, and you should be able to create a new notebook. 
This is a Python programming environment, and should allow you to send programming instructions to the Pixhawk.

### Use dronekit to send commands to Pixhawk
To connect to the Pixhawk vehicle, type the following into a cell in the notebook:

```
from dronekit import connect
vehicle = connect({serial_port}, baud=57600)
```

followed by shift+enter to execute the code.
After this, vehicle will be an object representing the Pixhawk, and can be communcated with via `vehicle.{function}`, where function is any function that the vehicle object contains.
To see a list of possible functions, type `vehicle.`, followed by tab.

Now you should be able to run any of the examples in http://python.dronekit.io/examples/index.html

For more information on connnecting to the vehicle, look at http://python.dronekit.io/guide/connecting_vehicle.html