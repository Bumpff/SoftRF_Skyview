# SkyView WAGA Device (softRF)

Note: I'm new to GitHub and C++ programming!

This project began in Feb 25 as a development of the SoftRF Skyview EZ display.

The intent of the project is to develop a companion display device for the WAGA Flarm.  WAGA is the Western Australia Gliding Association.  The WAGA Flarm is a not for profit low cost genuine Atom PowerFlarm with ADSB using a bespoke PCB designed and assembled in Perth, Australia.  Approx 35 have been built for WAGA members for use in gliders and tugs.

The starting point for this code was Moshe Fork revision M06b which was based upon Lyusupov revision 0.12.

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
    between 800-1600ms after alarm detection.  This delay is a characteristic of the e-paper screen.
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

![image](https://github.com/user-attachments/assets/1fffdee7-8316-4879-94eb-7f76a530750f)

![image](https://github.com/user-attachments/assets/cc4b385d-f22b-4fa8-9776-2aff10189efb)

![image](https://github.com/user-attachments/assets/d080e4e0-be98-4a6c-b9bd-030a0b344de0)

![image](https://github.com/user-attachments/assets/00375668-9db7-46d9-9b93-496ed2e365c4)



