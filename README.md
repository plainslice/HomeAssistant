# Home Assistant Configuration

This is a brief introduction of my Home Assistant (HASS) setup, so far. It is a work in progress, so I am constantly tweaking and adding devices and features. My goal for my home automation setup is to minimize the need to interact with the frontend. 90% of the automations are done, well...automatically. When I do interact with it manually, it is usually done on my android phone (more on that later).

**System Setup**

I am running HASS on a Raspberry Pi 3 (specifically, [this](https://www.amazon.com/gp/product/B01C6Q2GSY/ref=oh_aui_search_detailpage?ie=UTF8&psc=1) kit from amazon) via a [Hassbian 1.1](https://home-assistant.io/docs/hassbian/installation/) install. Hassbian 1.1 is the absolute easiest way to get up and running quickly. I have it installed on a 16gb card, as I find that to be the ideal size that allows quick system backups.

**Devices**
- 1 [Aeotec Z-Stick Gen. 5](https://www.amazon.com/Aeotec-Aeon-Labs-ZW090-Stick/dp/B00X0AWA6E/ref=sr_1_1?ie=UTF8&qid=1488061679&sr=8-1&keywords=aeotec+z+stick)
- 1 [Schlage Connect Z-Wave lock](https://www.amazon.com/Schlage-Touchscreen-Deadbolt-Technology-BE468-2K/dp/B01EKA88ZE/ref=sr_1_4?ie=UTF8&qid=1488061786&sr=8-4&keywords=schlage+connect+z+wave)
- 15 [GE Z-Wave Toggle Switches](https://www.amazon.com/dp/B00PYMGOHM/ref=twister_B01NANK0GL?_encoding=UTF8&psc=1)
- 1 [Aeotec Z-Wave Smart Strip](https://www.amazon.com/Aeotec-Aeon-Labs-DSC11-Sockets/dp/B00H3RL6JW/ref=sr_1_1?s=hi&ie=UTF8&qid=1488061854&sr=1-1&keywords=aeotec+smart+strip) that controls bedside lights
- 1 [GE Z-Wave smart outlet](https://www.amazon.com/GE-Lighting-Receptacle-Wireless-12721/dp/B0013V1SRY/ref=sr_1_1?s=hi&ie=UTF8&qid=1488061883&sr=1-1&keywords=zwave+outlet+ge) that controls two lamps sitting on the mantel
- 1 [Fibaro Z-Wave LED Controller](https://www.amazon.com/Fibaro-Micro-Controller-Z-wave-Strips/dp/B00P1N68FW/ref=sr_1_1?s=hi&ie=UTF8&qid=1488061904&sr=1-1&keywords=fibaro+zwave) that controls an LED strip used as a nightlight
- 2 [Ecolink Z-Wave Door / Window sensors](https://www.amazon.com/Ecolink-Intelligent-Technology-Operated-DWZWAVE2-ECO/dp/B00HPIYJWU/ref=sr_1_1?s=hi&ie=UTF8&qid=1488061924&sr=1-1&keywords=z+wave+door+sensor) used to control lighting in the basement and garage
- 2 [PIR Z-Wave motion sensors](https://www.amazon.com/Ecolink-Z-Wave-Motion-Detector-PIRZWAVE2-ECO/dp/B00FB1TBKS/ref=sr_1_1?ie=UTF8&qid=1488061980&sr=8-1&keywords=z+wave+motion) to control lighting automation
- [Ecobee 3](https://www.amazon.com/Ecobee3-Thermostat-Sensor-Generation-Amazon/dp/B00ZIRV39M/ref=sr_1_1?s=hi&ie=UTF8&qid=1488062016&sr=1-1&keywords=ecobee) used to control heating and A/C
- Liftmaster 8500: This comes with the MyQ Wifi gateway built in, and works perfectly with HASS
- Original Hue hub and two lights (not currently being used)

**Additional Software and Scripts**
- [Open Z Wave](https://github.com/home-assistant/hassbian-scripts#the-included-scripts): Needed for Z-Wave devices, easily installed via  hassbian script
- [HDMI CEC](https://github.com/home-assistant/hassbian-scripts#the-included-scripts): Used to turn off TV, easily installed via  hassbian script
- [Dasher](https://www.youtube.com/watch?v=qZpJ9W0wCks): I have a dash button near the bed used to set scene.goodnight
- [Let's Encrypt & DuckDNS](https://www.youtube.com/watch?v=BIvQ8x_iTNE): Access the front end using SSL
- [Github Backup](https://home-assistant.io/cookbook/githubbackup/): I back up all of my config files directly to Github using this script
- [Atom](https://atom.io/): Awesome free Mac program to edit yaml that works great along with these plugins:
    - [Remote FTP](https://atom.io/packages/Remote-FTP): SFTP program to connect easily to Pi
    - [YAML Sortkey](https://atom.io/packages/yaml-sortkey): Automatic YAML validation
- [iTerm 2](https://www.iterm2.com/): Great free alternative to Terminal that makes SSHing easier thanks to advanced features
- [Etcher](https://etcher.io/): Awesome app for imaging SD cards

**Automations**

*Presence Detection*

I experimented with various methods of presence detection, such as Owntracks and NMAP, in order to trigger automations before finally settling on just using [Bluetooth](https://home-assistant.io/components/device_tracker.bluetooth_tracker/). Owntracks was not very reliable in detecting when I got home, in addition to being a drain on my phone battery. NMAP did not seem to be very fast to pick up changes. I have found that Bluetooth, which Raspberry Pi 3 comes with, works perfectly on its own and has no noticeable effect on battery. It will detect my arrival as soon as I pull in (the range is quite impressive) and detect me leaving within 1-2 minutes, at most.

*Scenes*
- Gooddnight: At bedtime, turn off all lights, turn off TV, close garage, lock door, turn on bedside LED strip
- Goodbye: When the house has been empty for 10 minutes, turn off all lights, turn off TV, close garage, lock door
- Hello: When we arrive home after sunset, turn on lights
- Sunset: When the sun sets when we are already home, turn on lights
- Ambience: Turn off bedroom light and turn on bedside lights (manually triggered, see Android section)
- Morning: Turn off LED strip and turn on office and kitchen lights when I wake up (triggered via MacroDroid, see Android section)

*Motion / Door Sensors*
- Basement Door: When basement door is open, turn on basement light. Turn off when door is closed
- Garage Light: When door to garage is opened (and the garage is closed, i.e., when we are leaving house), turn on garage light for 5 minutes
- Garage Entry: When motion is sensed in garage hallway after sunset, turn on garage hallway light for 5 minutes
- Stair Lights: When motion is sensed in bedroom hallway, between 10pm and 6am, turn on stair lights at low brightness for 5 minutes

*Notifications* (All notifications done via [Join](https://joaoapps.com/join/))
- Push notification when any battery powered devices reaches 10%
- Push notification when automation.Goodbye is triggered
- Push notification when automation.Hello is triggered
- Push notification when Z Wave network is back up, after restart (extremely helpful!)

**Android Setup**

Most of my manual interaction with HASS is done via my Android phone (Pixel), either through quick settings tiles, Macrodroid, or Google Assistant.

*Macrodroid Automations*

I wanted to find a better way to turn lights on in the morning, since I wake up at different times depending on the day, and often before my alarm goes off. Using a set time did not work.

I found an optimal solution using Macrodroid on my phone, using the HTTP Shortcuts app to trigger a POST request to turn on scene.morning. Macrodroid will trigger the request whenever my alarm notification is dismissed, either from going off or me dismissing it early (what usually happens).

*Google Assistant*

Via IFTTT, I can trigger various automations, using Google Assistant.
Some commonly used ones are "Ok Google, close the garage" and "OK Google, goodnight" (trigger automation.goodnight).

*Quick Settings Tiles*

Below is what my quick tiles look like on my phone. I have tiles for my three most used scenes that I found myself opening up HASS to activate. Using quick tiles is much quicker to do. [Here](https://github.com/pizzapants/HomeAssistant/blob/master/android.md) are detailed instructions on how I set that up, using Macrodroid and HTTP Shortcuts.

![Quick Tiles](http://i.imgur.com/ZRjMp85.png)

**Front End**

*Default View: Overall Status*
![Default View](http://i.imgur.com/CYcH1Uw.png)

*First Floor: Living Room & Kitchen / Basement & Garage*
![First Floor](http://i.imgur.com/FqaxiKg.png)

*Second Floor: Bedroom & Office*
![Second Floor](http://i.imgur.com/elmpNcF.png)

*Ecobee Status*
![Third Floor](http://i.imgur.com/5ZyVBuf.png)

*Automations*
![Automations](http://i.imgur.com/oLRxp20.png)

*Settings*
![Settings](http://i.imgur.com/CnoJReT.png)
