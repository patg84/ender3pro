Last updated: 11.06.24

# Pat's Modified Ender 3 Pro
Klipper Configs & How-To Guides for Ender 3 Pro



# After Klipper config has been tuned...

1. Edit printer.conf to liking...
2. Disable Stealth Chop for more accurate results in Z axis.



# Initial Calibration Order After Changing Parts:

1. Heat bed to 60°C for PETG (Garolite surface) (nozzle will be at 190°C, for my reference)
2. Move > Home > Home All
3. Disable motors and pull bed forward
4. Loosen thumbwheels until they free spin > spin counterclockwise until they stop > align blue tape to front > screw clockwise X turns > back off 1 turn or if using springs, screw down thumb wheels until they stop, then back off one turn.
5. G28
6. Replace nozzle if needed
8. Heat hot end (make sure nozzle face is clean prior)
9. More > Z Calibrate (Set Probe Z Offset) > Start > Ready Post-it note > slide under nozzle while making Z adjustment (want slight drag on paper) > Accept & Reboot
10. Heat bed to 60°C
11. Move > Home > Home All
12. Move > Home > Z Tilt (Run Z_TILT_ADJUST) (calibrates each Z motor, verify in console for retries. Usually takes about 6 tries to get under 0.02500)
13. G28
14. SCREWS_TILT_CALCULATE (wouldn't this come before z tilt adjust?). Found that a minor adjustment was needed but no more than 0 retries. Ran twice to confirm.
15. More > Bed Mesh > Calibrate (homes then probes 9 points) > Save and Reboot
17. Calibrate Hot End PID (Temperature > Tap current extruder temp > Type in 190 > Tap "Calibrate PID" > Accept time warning > Accept & Reboot)
18. Calibrate Bed PID (Temperature > Tap current bed temp > Type in 60 > Tap "Calibrate PID" > Accept time warning > Accept & Reboot)
19. ADXL345 for both X and Y axes
20. Print test cube
21. Adjust Z height on the fly



# Additional Calibration Steps for Accurate Prints:

1. Calibrate e-Steps via the hot method (https://ellis3dp.com/Print-Tuning-Guide/articles/extruder_calibration.html)
2. 



# To Do:

1. Now I just need to figure out how cancel a print and have the Z axis raise up automatically. Klipper wants to reboot after a failed print and it cuts the power to the Z steppers which drops the hot nozzle into the part.
2. Filament Sensor using original Ender Z stop switch.


# OrcaSilcer Important Notes:

1. Download and install newest version from (https://github.com/SoftFever/OrcaSlicer/releases)
2. Extract to folder and run.
3. Saved profile data will load from here (%APPDATA%\OrcaSlicer) AKA (C:\Users\Pat\AppData\Roaming\OrcaSlicer)



# Use this to tune Klipper:
(https://ellis3dp.com/Print-Tuning-Guide/)






# Pi Cheat Sheet Commands:

Shutdown orange pi immediately:      sudo shutdown -h now
