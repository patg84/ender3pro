# Klipper Resonance Tester - ADXL345 Linux Build Process:

## 📌 Current ADXL345 Wiring & Configuration

Currently using (2) ADXL345 devices wired in parallel back to the GPIO header on the OPZ3 and using a 6a 125ac SPDT 3-position mini switch to toggle between the Hot End ADXL345 chip and the Bed ADXL345 chip. Middle position is off. Switch breaks 3.3v connection between the OPZ3 and each ADXL345. Couldn't figure out device SPI overlays for the OPZ3 in Debian so I resorted to the manual switch. This way I know only one chip is active at a time and I can manually cut the power to both when done with the calibration. Need to design a permanent mount for the bed sensor.

All wires are crimped using dupont connectors.

## 📌🧪 Prerequisite for Getting the Linux Host Process to Run on an Orange PI (OEM Debian OS):

https://klipper.discourse.group/t/armbian-kernel-klipper-host-mcu-got-error-1-in-sched-setschedule/1193 | https://chatgpt.com/c/670dedfa-7a90-8006-b17e-46cd8f1e2e09

1 - Locate kernel configuration file:

    ls /boot/config-$(uname -r)

2 - Check if CONFIG_RT_GROUP_SCHED is enabled (if enabled stop here):

    grep CONFIG_RT_GROUP_SCHED /boot/config-$(uname -r)

3 - Test this fix before commiting on step 4 (resets on reboot)    
    
    sudo sysctl -w kernel.sched_rt_runtime_us=-1                                                            # Disables realtime scheduling for testing, will reset on reboot

4 - Permanant Fix:

    echo "kernel.sched_rt_runtime_us = -1" | sudo tee /etc/sysctl.d/10-disable-rt-group-limit.conf          # Disable real time scheduling
    
    sudo sysctl -p /etc/sysctl.d/10-disable-rt-group-limit.conf                                             # Apply the setting immediately

5 - Reboot and check state (should be enabled):

    grep CONFIG_RT_GROUP_SCHED /boot/config-$(uname -r)

6 - Proceed with ADXL345 Setup...


## 📌How to Setup and Add the ADXL345 to Klipper:

https://www.klipper3d.org/Measuring_Resonances.html

Requires 2 ADXL345 chips (x, y) that you'll wire in and flip control with a SPDT switch.



## 📌Enable SPI Interface in the OS Settings:

    sudo orangepi-config

      System --> Hardware --> Make sure nothing is selected
      System --> Bootenv

    Add the param_spidev line below:

    verbosity=1
    bootlogo=false
    console=both
    disp_mode=1920x1080p60
    overlay_prefix=sun50i-h616
    rootdev=UUID=635d9db0-d9c8-453e-baca-8b0e028d5dcf             # Random UUID - Do not change
    rootfstype=ext4
    param_spidev_spi_bus=1                                        # <------------ Add this line only!
    usbstoragequirks=0x2537:0x1066:u,0x2537:0x1068:u              # This line gets added later automatically

  Save, Back, Exit, and Reboot



## 📌Install Required Software on Orange PI First:

    sudo apt update
    
    sudo apt install python3-numpy python3-matplotlib libatlas-base-dev libopenblas-dev

    ~/klippy-env/bin/pip install -v numpy                         # Installs NumPy




## 📌Build Host Process File for ADXL345 on Orange PI:

    cd ~/klipper
    
    make menuconfig

      (*) Enable extra low--level configuration options
      Micro-controller Architecture (Linux Process)               # Change from "STM32" to "Linux Process"
      ( ) GPIO pins to set at micro-controller startup

    Press "Q" then "Y" to save.

    make clean                                                    # Clears out dir - /home/orangepi/klipper/out/
    
    make                                                          # Build ADXL345 linux process file



## 📌Add the Following to the printer.cfg File:

    # ADXL345 Input Shaper Section:
    #
    # # https://www.klipper3d.org/Measuring_Resonances.html
    #
    # Added by PG
    #  Make sure (sudo orangepi-config) has "param_spidev_spi_bus=1" in the "u-boot env" section and that SPI is not enabled in the hardware section!! This is the SPI1 bus as seen on orangepi gpio diagrams.
    #
    # *** Run automatic or manual calibration but not both! ***
    # 
    # Find the MCU device by path or id (path is preferred):
    # 
    #    ls /dev/serial/by-path/*
    #    ls /dev/serial/by-id/*
    #
    # Automatic calibration of each axis (https://www.klipper3d.org/Measuring_Resonances.html#input-shaper-auto-calibration):
    #
    #    1. Run "ACCELEROMETER_QUERY" to get current ADXL345 data and verify ADXL345 chip is connected and working correctly.
    #    2. Run "SHAPER_CALIBRATE AXIS=X" to run test
    #    3. Run "SAVE_CONFIG" to save data to printer.cfg
    #    4. Run "SHAPER_CALIBRATE AXIS=Y" to run test
    #    5. Run "SAVE_CONFIG" to save data to printer.cfg
    #
    # Manual calibration of each axis (https://www.klipper3d.org/Measuring_Resonances.html#testing-custom-axes):
    #
    #    1. Run "ACCELEROMETER_QUERY" to get current ADXL345 data and verify ADXL345 chip is connected and working correctly.
    #    2. Run "TEST_RESONANCES AXIS=X' and "TEST_RESONANCES AXIS=Y" to run test and add CSV data to "/tmp" on the Pi. Once data is aquired for both axes run the following command to get recommended settings and PNGs of graphs:
    #    3. ~/klipper/scripts/calibrate_shaper.py /tmp/resonances_x_*.csv -o /tmp/shaper_calibrate_x.png
    #    4. ~/klipper/scripts/calibrate_shaper.py /tmp/resonances_y_*.csv -o /tmp/shaper_calibrate_y.png
    #    5. Recommended input shaper data will be displayed. Add this to the INPUT_SHAPER section of printer.cfg

    [mcu rpi]
    serial: /tmp/klipper_host_mcu          # This is the driver file.

    [adxl345]
    cs_pin: rpi:None
    spi_bus: spidev1.1                     # To get this value run, (ls /dev/spi*) to see available SPI devices.

    [resonance_tester]
    accel_chip: adxl345                    # Specifies the type of accelerometer chip being used.
    probe_points:                          # As per Aubey in Discord, this is where the tool head should park when running the test.
        100, 100, 20  # Set by PG          # This should be the middle of the bed (x, y, z)




## 📌Save the printer.conf in the Klipper GUI & Restart the host (Orange PI)



👽
