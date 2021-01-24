# SofarCtrl
## An MQTT modbus interface for the Sofar ME3000SP solar battery inverter.
![SofarCtrl](pics/SofarCtrlOn.jpg)

SofarCtrl is a modbus interface for the Sofar ME3000SP solar battery inverter.
It allows remote control of the ME3000 in passive mode via incoming MQTT messages and reports
back the invertor status, power usage, battery state etc via outgoing MQTT messages.
It's designed to run on an ESP8266 microcontroller with a TTL to RS485 module such as MAX485 or MAX3485.
Also compatible with HYD ES Series tested on the HYD-4000-ES.

Subscribe your MQTT server to these queues:

sofar/running_state  
sofar/grid_voltage  
sofar/grid_current  
sofar/grid_freq  
sofar/battery_power  
sofar/battery_voltage  
sofar/battery_current  
sofar/batterySOC  
sofar/battery_temp  
sofar/battery_cycles  
sofar/grid_power  
sofar/consumption  
sofar/solarPV  
sofar/today_generation  
sofar/today_exported  
sofar/today_purchase  
sofar/today_consumption  
sofar/inverter_temp  
sofar/inverterHS_temp  
sofar/solarPVAmps  

Send MQTT messages to these queues:

sofar/standby   - send value true or false  
sofar/auto   - send value true, false or battery_save  
sofar/charge   - send value in the range 0-3000 (watts)  
sofar/discharge   - send value in the range 0-3000 (watts)  

battery_save is a hybrid auto mode that will charge from excess solar but not dischange.

(c)Colin McGerty 2021 colin@mcgerty.co.uk

# How To Build

Parts List:
1. ESP8266 Microcontroller
2. MAX485 or MAX3485 TTL to RS485 board
3. Wemos 64x48 OLED Screen (optional)
4. A small project board
5. A few wires and a little solder

![Parts](pics/parts.jpg)

Cut the project board to a convenient size.

![Board](pics/board.jpg)

Wire the componets according to this circuit diagram.

![Wiring Diagram](pics/diagram.jpg)

I tend to keep the wires on top of the board, poke them through and solder underneath. Your approach may be better and your soldering may way be better than mine!

![Wiring](pics/wiring.jpg)

Make sure you connect the DR and RE pins together. The red arrow below shows where a single wire from D5 connects to both DR and RE.
This step is not necessary if you use an Auto Flow RS485 module with RX and TX pins.

![Short these pins](pics/short.jpg)

Use long pinned mounts on your ESP8266 if you are stacking the optional OLED on top.

![Chips](pics/ICs.jpg)

Here's how it looks when completed.

![Finished](pics/SofarCtrl.jpg)

# Flashing

Open SofarCtrl.ino in the Arduino IDE.

Edit SofarCtrl.ino with your wifi network name and password and your mqtt server details. 

You'll need the libraries for the ESP8266. Follow [this guide](https://randomnerdtutorials.com/how-to-install-esp8266-board-arduino-ide/) if you haven't completed that step before.

Add a few more libraries using the Manage Libraries menu:
1. PubSubClient
2. SoftwareSerial
3. Adafruit GFX
4. Adafruit SSD1306 Wemos Mini OLED

(Even if you are not using the OLED screen, you should install the Adafruit libraries or it will not compile.)

...and upload.

When you run it on the desktop, not connected to you invertor, you can test that wifi and mqtt are connected at start up.
The OLED screen should show "Connected". It should also show "RS485 Error" to indicate that the connection to the interter is not there.

# Connect to Inverter

Connect the SofarCtrl unit to a 5v micro USB power supply.
Now connect wires A and B to the two wire RS485 input of your ME3000SP, which is marked as 485s on the image below.

![ME3000SP Data Connections](pics/485s.jpg)


