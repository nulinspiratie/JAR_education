# Setup of Raspberry Pi
This guide is written to setup the Raspberry Pi for use with the Pixhawk to control a drone.
At the end of the guide, the Raspberry Pi will be accessible from a laptop in two ways:
	1. **VNC** allowing the full display of the Raspberry pi to be displayed directly on the laptop
	2. **Jupyter notebook**, allowing Python code to be executed easily from a webbrowser on the laptop, allowing easy control of the drone.

While in principle all Raspberry Pi's are compatible with the PixHawk, the Raspberry Pi Zero W is recommended because of its small size and cheap cost.
Therefore, this guide is written for configuration of the Raspberry pi Zero W.

It should also be possible to copy an image of the SD card OS to other SD cards.
This would mean that the following configuration only needs to be run once, and the resulting SD card image can be copied over to other SD cards, thereby avoiding to repeat the same configuration for each Raspberry Pi.
This has not been tested yet.


## List of equipment
Below is a list of equipment needed to set up the Raspberry Pi zero W to be used with PixHawk to control a drone.

- Raspberry pi zero W.
- microSD card with NOOBS installed.
	- Can be bought with NOOBS pre installed.
	- If using empty SD card, follow instructions on [https://www.raspberrypi.org/downloads/]
- Break-away 0.1 2x20-pin Strip Dual Male Header (GPIO pins)
- Mouse and keyboard (usb connectivity)
- Multi-usb hub with micro-USB connector (so both a mouse and keyboard can be used simultaneously)
- Mini-HDMI to HDMI converter and HDMI cable
- Screen (monitor or TV) with HDMI port
- GPIO pins
- micro-USB to USB cable, and USB connector power adapter
- Laptop or PC, preferrably one that supports creating a Wi-Fi hotspot
- Wi-Fi connectivity

## Initial setup of Raspberry Pi
1. Insert microSD card into Raspberry Pi slot
2. Connect screen, mouse, keyboard to Raspberry Pi
	- For Raspberry Pi zero, connect USB hub to left micro-USB connector, and attach mouse and keyboard to hub.
3. Connect micro-USB cable to Raspberry Pi and connect other side to power outlet. The Raspberry Pi should power up.
	- For Raspberry Pi zero, use right micro-USB connector for power.
4. Select Raspbian when asked what OS to install.
5. After installation is complete, restart and login.
	- username=`pi`
	- password=`raspberry`
6. Connect to Wi-Fi in top-right cornner


## Installing necessary packages
Packages are needed to connect to the Pixhawk, to enable VNC, and to use Jupyter notebook.
Packages can be installed using the terminal command line.
In the following instructions, if asked for confirmation, press `Y` followed by `enter`.

1. Open terminal (black box at the top of the desktop)
2. Run `sudo apt-get update`
3. Run `sudo apt-get install screen python-wxgtk2.8 python-matplotlib python-opencv python-pip python-numpy python-dev libxml2-dev libxslt-dev`
4. Run `sudo pip install --upgrade pip`
5. Run `sudo pip install future jupyter droneapi  dronekit dronekit-sitl`
	- This step can take tens of minutes


## Setting up VNC
Setting up VNC allows you to view the Raspberry Pi display from your laptop/computer throuh Wi-Fi.
Instructions for this stage can be found on [https://www.realvnc.com/en/raspberrypi/]

Some routers do not support VNC, or there may not be a Wi-Fi connection where you want to fly the drone.
It is therefore recommended to create a Wi-Fi hotspot on the laptop/pc, and use it for the VNC connection (see optional step 4).
This way, you can be reasonably sure that your drone will remain connected to the laptop, provided they are in reasonable proximity of one another.

### Part I: Setting up VNC on Raspberry Pi
1. Open terminal
2. Run `sudo apt-get install realvnc-vnc-server realvnc-vnc-viewer`
2. On the Raspberry Pi Dekstop, select `Menu (Raspberry icon) > Preferences > Raspberry Pi Configuration > Interfaces`
3. Click `Enabled` for VNC.
4. (Optional) Create Wi-Fi hotspot on laptop/pc and connect Raspberry Pi to it.
5. Click on the blue VNC icon that should appear in the top right.
6. take note of the ip address on the VNC screen

### Part II: Connecting to Raspberry Pi from laptop/pc
1. Download VNC viewer from [RealVNC](https://www.realvnc.com/en/raspberrypi/).
2. Enter ip address of Raspberry Pi and connect
	- Username=`pi`
	- password=`raspberry`

## Setting up Jupyter notebook server to run on startup
Jupyter notebook allows you to run Python code interactively through a web browser.
By creating a notebook server on the Raspberry Pi, it can be accessed via a webbrowser on the laptop.
Here we will configure the server to run every time the Raspberry Pi starts up.

1. Set kernel password
	I. Open terminal 
	II. Run: `jupyter notebook password`
	III. Choose and type password when requested followed by `enter`
	IV. Retype password and press `enter` again
2. Create service to start kernel during startup
	I. In terminal, run: `sudo nano /usr/lib/systemd/system/jupyter.service`.
		This will open up an empty text editor.
	II. Copy the [jupyter.service](###Appendix:jupyter.service) code into the terminal.
	III. Save and exit
		A. press `ctrl+w` to exit
		B. press `Y` when it asks if you want to save
		C. press `enter` when it asks for a name
	IV. Run: `sudo systemctl enable jupyter.service`
	V. Run: `sudo systemctl daemon-reload`
	VI. Run: `sudo systemctl restart jupyter.service`
	VII. Test if it's working
		A. Open webbrowser on laptop
		B. Navigate to `<ip_address>:8888` (replace `<ip_address>` with IP address found earlier)
		C. If successfull, it will ask for a password, which has been set previously.
		
You should now be able to access jupyter notebook that's running on the Raspberry pi from your laptop.
When configured with the PixHawk, this also means you can interactively send commands to your drone.

Though it shouldn't be necessary, you can check the output of the notebook server by typing `sudo journalctl -b -u jupyter` into the terminal.

## Configuring Raspberry Pi for the PixHawk
Detailed instructions can be found on [http://ardupilot.org/dev/docs/raspberry-pi-via-mavlink.html].
This section has not been tested with the Pixhawk yet.

1. Open terminal
2. Run `sudo raspi-config`
3. Go to `5 Interfacing Options`
4. Select `P6 Serial`
5. Select `No` to `login shell`
6. Select `Yes` to `serial port`
7. Press `esc` to exit


### Appendix:jupyter.service
```
[Unit]
Description=Jupyter Notebook

[Service]
ExecStart=/usr/local/bin/jupyter notebook --ip=* --no-browser
User=pi
WorkingDirectory=/home/pi/
Restart=always

[Install]
WantedBy=multi-user.target
```


