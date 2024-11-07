# Pat's Modified Ender 3 Pro
Klipper configs & How-Tos for Ender 3 Pro

Last updated: 11.06.24



# After klipper config has been tuned...

1. Edit printer.conf to liking...
2. Disable Stealth Chop for more accurate results in Z axis.



# Initial Calibration Order After Changing Parts:

1. Heat bed to 60°C for PETG (Garolite surface) (nozzle will be at 190°C, for my reference)
2. Screw down thumb wheels, then back off one turn
3. G28
4. Replace nozzle if needed
5. Set Probe Z Offset
6. Run Z_TILT_ADJUST
7. G28 (runs automatically)
8. SCREWS_TILT_CALCULATE
9. Bed mesh
10. Save and reboot
11. PID Hot End Tune
12. PID Bed Tune
13. ADXL345 for both X and Y axes
14. Print test cube
15. Adjust Z height on the fly



# Additional Calibration Steps for Accurate Prints:

1. Calibrate e-Steps



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
