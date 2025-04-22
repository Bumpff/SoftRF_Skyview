# SkyView WAGA Device (softRF)

Note: I'm new to GitHub and C++ programming so this may not meet normal standards!

This project began in Feb 25 as a development of the SoftRF Skyview EZ display.

The intent of the project is to develop a companion display device for the WAGA Flarm.  WAGA is the Western Australia Gliding Association.  The WAGA Flarm is a not for profit low cost genuine Atom PowerFlarm with ADSB using a bespoke PCB designed and assembled in Perth, Australia.  Approx 35 have been built for WAGA members for use in gliders and tugs.

NOTE:  The SkyView hardware is a Lilygo T5S which is an ESP32 with WaveShare 2.7" e-paper screen.  The e-paper screen is probably the limiting factor for this project.  Firstly , the screen datasheet says it's not suitable for use in strong sunlight - but its cheap to replace.  Then there is an issue re speed of screen update.  The code uses 'fast update' for the screen which I have measured as taking approx 850ms. However, the 'next screen' buffer is not available for writing to during a 'fast' update.  So, the worst case is an alarm received just after an update has initiated - the alarm cannot be passed to the screen for 850ms, and then it will take another 850ms to update and become visible.

The code has been tested using PowerFlarm and a traffic simulator (details below).  It probably still works with SoftRF transceivers but I've not tested it and as its not relevant to this project, I wont be fixing any related issues.

The starting point for this code was Moshe Braner's Fork revision M06b, which was based upon Linar Lyusupov revision 0.12.

Additions and changes to MB06B include:
======================================
1.  Landscape screen layout added to increase installation flexibility.  Use the WebGUI to select landscape or portrait.  (Does not do 180 rotation like Linar v0.13)
2.  NavBoxes in Radar_View amended to:  Threat, Traffic number, Vertical Distance and CompID (looked up from OGN database on SD card).
3.  Nav_View mode added.  Provides basic navigation data to hardcoded Waypoints for use by tugs on aero retrieves. Same radar display
  as RadarView.  NavBoxes are: Threat, Bearing to waypoint, Distance to waypoint and Ground Speed. Use WebGUI to select waypoint.
4.  There is no battery level indicator because this device will be hardwired to power and data.  Code remains unchanged.
5.  Additional fields added to NMEA parse: 'Notrack' and 'Source' (available from Flarm protocol 9 onwards). 'Source' distinguishes ADSB and mode-s traffic.
6.  Added concept of 'Threat' where threat is the current highest Alarm Level or closest traffic if no alarms.
7.  Threat NavBox1 shows Threat distance or Alarm Level. NavBox 4 shows Comp ID of Threat (the Hex code is visbible in small font).
8.  Alarm 'lookout relative bearing indicator' added.  When a Level 1 Alarm is detected, a black triangle is shown just inside the scale ring showing the pilot where
   to look for the Threat. If a Level 2 Alarm is detected, a triangle is added on the same arc as the black triangle but closer to the radar centre.
  If a Level 3 Alarm is detected, a further triangle is added.  Note: all triangles indicate the direction to look for the traffic, not the the traffic location on
  the radar.  Direction of lookout indicator is always relative to ThisAircraft heading, even when radar in North-Up mode.
9. Revised WebGUI option regarding Voice.  Now 'Sound' options are:  Off, Voice or Buzzer.  Note the SkyView WAGA has a buzzer installed using GPIO Pin 10.
10. Voice is Male voice for advisories "Traffic".  Female fast voice for alarms in ascending order are; "Alert", "Warning", Danger" with X 0'clock and above/below/level.
11. Buzzer on occurs immediately an alarm is detected. Buzzer off and repeat interval is triggered by loop events so as not to add delays to the loop. Buzzer is irregular.
12. Threat traffic has a line drawn to it from ThisAircraft.  Tugs have a single ring around the traffice icon. ADSB if detected has a double ring around the icon.
13. Non-directional traffic, if detected using Source, is shown as a ring of dots. Voice states "Aware" instead of X o'clock.
14. The delay between screen updates is set as 2000ms (as per MB06B).  However, Alarms are processed as priority when received and thus screen display is improved to
    between 850-1700ms after alarm detection.  This delay is a characteristic of the e-paper screen.
16.  On startup:  If setting=Voice the voice 'post' jingle is suppressed.  If Setting=Buzzer, 2 buzzes are made.
17.  On startup, if 'No data' is available, message shows what connection and baud rate is set.  If 'No Fix', message shows what type of data is being received (eg NMEA).
    This information previously was shown in the NavBoxes.
19.  A user manual has been provided.

I have also developed a tool using MS Access for creating and sending traffic simulations.  It's proven invaluable during testing.  It's a working tool, not a 'polished' programme!
Simulations using PLAU and PFLAA sentences can be created and saved as data tables.
GPS fix data can be inserted.  CheckSums calculated and concated to the Sentance.
Then directly from MS Access (using USB->RS232_TTL conversion), at the push of a button, the sim is fed at the required input rate to the Skyview.
The PowerFlarm built in sims are in the database.
The tool can also be used to communicate with a PowerFlarm to read the settings and initiate the built-in sims. 

![image](https://github.com/user-attachments/assets/d60e1500-8a59-4c47-92f7-ea86ffdb929e)

![image](https://github.com/user-attachments/assets/a36aabe8-a0b7-445a-a084-25a9363579ed)

![image](https://github.com/user-attachments/assets/0452630e-c46c-42e8-9fae-4a9853a871e2)

![image](https://github.com/user-attachments/assets/88cc1f16-41d7-473b-bf7f-df6781b72471)

![image](https://github.com/user-attachments/assets/a5d38270-ffc8-4bcc-a441-61fc425fd417)

For discussions join the SoftRF Community.

For additional info see also Lyusupov's repository   https://github.com/lyusupov/SoftRF/tree/master/software/firmware/source

and 

Moshes repository:   https://github.com/moshe-braner/SoftRF

