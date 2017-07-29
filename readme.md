# Raspberry Pi installation
While in principle all Raspberry Pi's are compatible with the PixHawk, the Raspberry Pi Zero W is recommended.


## List of equipment
Below is a list of equipment needed to set up the Raspberry Pi zero W to be used with PixHawk to control a drone.

- Raspberry pi zero W.
- Mouse and keyboard (usb connectivity)
- Multi-usb hub with micro-USB connector (so both a mouse and keyboard can be used simultaneously)
- microSD card with NOOBS installed.
	- Can be bought with NOOBS pre installed.
	- If using empty SD card, follow instructions on https://www.raspberrypi.org/downloads/
- Mini-HDMI to HDMI converter and HDMI cable
- Screen (monitor or TV) with HDMI port
- GPIO pins
- micro-USB to USB cable
- Laptop or PC, preferrably one that supports creating a Wi-Fi hotspot
- Wi-Fi connectivity

## Initial setup
1. Insert microSD card into Raspberry Pi slot
2. Connect screen, mouse, keyboard to Raspberry Pi
	- For Raspberry Pi zero, use left micro-USB connector for USB hub
3. Connect USB cable to PC and to Raspberry Pi to power up
	- For Raspberry Pi zero, use right micro-USB connector for power
4. Select Raspbian when asked what OS to install
5. After installation is complete, restart and login
	- username=pi, password=raspberry
6. Connect to Wi-Fi in top-right cornner

## Setting up Python
Once 
python 2.7

### Installing necessary packages
If asked for confirmation, press Y and enter
sudo apt-get update
sudo apt-get install screen python-wxgtk2.8 python-matplotlib python-opencv python-pip python-numpy python-dev libxml2-dev libxslt-dev

sudo pip install --upgrade pip
sudo pip install future jupyter droneapi  dronekit dronekit-sitl
	- This step can take tens of minutes

Stop ip changing
chattr +i /etc/resolv.con

http://ardupilot.org/dev/docs/raspberry-pi-via-mavlink.html
Open terminal and type
	sudo raspi-config
	5 Interfacing Options
	P6 Serial
	No to login shell
	Yes to serial port  
	Press esc to exit


## Setting up VNC
Setting up VNC allows you to view the Raspberry Pi from your laptop/computer throuh Wi-Fi.
Instructions for this stage can be found on https://www.realvnc.com/en/raspberrypi/ 

Some routers do not support VNC , or there may not be a Wi-Fi connection where you want to fly the drone.
It is therefore recommended to create a Wi-Fi hotspot on the laptop/pc, and use it for the VNC connection (see optional step 4).

### Part I: Setting up VNC on Raspberry Pi
1. Open a terminal, and type
	sudo apt-get install realvnc-vnc-server realvnc-vnc-viewer
2. On the Raspberry Pi Dekstop, select Menu (Raspberry icon) > Preferences > Raspberry Pi Configuration > Interfaces.
3. Click Enabled for VNC.
4. (Optional) Create Wi-Fi hotspot on laptop/pc and connect to it on raspberry pi using Wi-Fi.
5. Click on the blue VNC icon that should appear in the top right.
6. take note of the ip address on the VNC screen

### Part II: Connecting to Raspberry Pi from laptop/pc
1. Download VNC viewer from realvnc website above.
2. Enter ip address of raspberry pi and connect
	- Username=pi, password=raspberry

## Setting up Jupyter notebook server to run on startup

1. set kernel password
	I. In terminal, run command: jupyter notebook password
	II. Type password when requested and press enter
	III. Retype password and press enter again
2. Create service to start kernel upon boot
	I. In terminal, run: sudo nano /usr/lib/systemd/system/jupyter.service
	II. Copy the following piece of code into the terminal, replacing <ip_address> with actual ip address

```
[Unit]
Description=Jupyter Notebook

[Service]
ExecStart=/usr/local/bin/jupyter notebook --ip=<ip_address> --no-browser
User=pi
WorkingDirectory=/home/pi/
Restart=always

[Install]
WantedBy=multi-user.target
```

	III. Save and exit
		A. press ctrl+w to exit
		B. press y when it asks if you want to save
		C. press enter when it asks for a name
	IV. Run: sudo systemctl enable jupyter.service
	V. Run: sudo systemctl daemon-reload
	VI. Run: sudo systemctl restart jupyter.service
	VII. Test if it's working
		A. Open webbrowser on laptop
		B. Navigate to <ip_address>:8888 (replace <ip_address>)
		C. If successfull, it will ask for password. This is the one previously set
		
You should now be able to access jupyter notebook that's running on the Raspberry pi from your laptop.
When configured with the PixHawk, this also means you can interactively send commands to your drone.