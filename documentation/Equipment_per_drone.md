# Equipment per drone
If an item is not ticked, it either means we don't have it at all, or we don't have one for each drone.


## General drone equipment
### Equipment that we only need one of
- [x] PPM encoder + RC receiver/tranmitter
	- We need at least one to be able to test the drones out.
	- Also, it might be good to have an RC transmitter/receiver as a safety to override any bad programming instruction (e.g. if a command was sent to continue flying in a direction indefinitely).
	- There is a chance that one is needed per drone, bec ause one of the arming checks is for an RC controller. However, I think this can be overridden

### Equipment per drone
- [ ] Drone case
- [ ] Pixhawk
- [ ] LiPo battery pack
- [ ] Power module
- [ ] 4 ESC motors
- [ ] 6-inch propellers
	- At least four per drone, probably more since they break easily
- [ ] GPS
- [ ] Rotors
- [x] Jumper lead cables
	- We should have sufficient of these already
- [ ] 5-pin DF13 to breakout connector
	- Must satisfy connectivity shown on http://ardupilot.org/dev/docs/raspberry-pi-via-mavlink.html
- [ ] 4x AAA batteries
- [ ] Case that can draw current from 4 AAA batteries.
- [ ] Potentially PPM encoder + RC receiver/tranmitter
	- This is probably not necessary (see above)


## Raspberry Pi equipment

### Equipment that we only need once
We either have all these items, or they can be easily arranged
- [x] HDMI cable
- [x] mini-HDMI to HDMI converter
- [x] USB hub
- [ ] keyboard and mouse
- [ ] Laptop that can create Wi-Fi hotspot
- [x] Soldering station
	- I could probably use the one in the lab
- [x] USB to micro-USB cable
- [ ] USB power adapter

### Equipment per drone
- [x] Raspberry Pi Zero W
	- W stands for Wi-Fi
- [x] microSD card 
	- maybe preferrable with NOOBS preinstalled, not sure yet
- [x] Break-away 0.1 2x20-pin Strip Dual Male Header
	- These GPIO pins need to be soldered onto the Raspberry Pi
- [ ] Raspberry Pi Zero case (optional)
	- Might make installing it onto the drone easier
