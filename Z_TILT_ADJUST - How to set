z_tilt]                            # Enable to activate dual or triple Z auto level in realation to the build plate.
z_positions:                       # Left and right Z motor locations in space. This will be off the build plate. (Ender 3 Pro, steppers are to the back left and back right while looking at the front of the unit).
                                   # Coordinates below are in x,y from the 0,0 point which is directly under the nozzle at the front left of the build plate (3mm in from the left and front of the build plate).
                                   # Make sure x,y values below are indented 4 spaces or this won't work.

####################################

# To get the X location value of the stepper's position follow this:

# This is the left stepper. Home and jog nozzle to 0,0. Nozzle will be in front left corner of build plate 3mm in from the left and 3mm back from the front of the plate. Raise Z axis by 10 so it doesn't slam into the bed for the next step. Look top down at left Z coupler and jog Z axis up until the cutout on left stepper coupler is in line with Y axis pointing backwards. Looking at the back side of the printer, measure with a mm ruler starting from 3mm in on the build plate. Line it up with the split in the left Z coupler. That's the X value, -29.5mm. It's a negative number because it's to the left of 0,0. in Cartesian coordinates when looking at the printer from the front.

# This is the right stepper. Home and jog nozzle to 0,0. Nozzle will be in front left corner of build plate 3mm in from the left and 3mm back from the front of the plate. Raise Z axis by 10 so it doesn't slam into the bed for the next step. Look top down at right Z coupler and jog Z axis up until the cutout on right stepper coupler is in line with Y axis pointing backwards. Looking at the back side of the printer, measure with a long mm ruler starting from the cutout on the right coupler across the build plate to 3mm in from the left side of the build plate. That's the X value, 258.5mm. It's a positive number because it's to the right and back of 0,0. in Cartesian coordinates when looking at the printer from the front.

####################################
    
# To get the Y location value of the stepper's position follow this:

# This applies to both the left and right stepper motors as long as they're in line with each other (Ender 3 Pro, Prusa, etc.). Home and jog nozzle to 0,0. Nozzle should be in front left corner of build plate 3mm in from the left and 3mm back from the front of the plate. Raise Z axis by 10 so it doesn't slam into the bed for the next step. Jog Z axis until cutout on left coupler on stepper aligns with X axis (point inward towards bed to get accurate measurement). Jog the X axis to the left by 30mm so the print head is out of the way. Look down at the build plate. Put a post it note or a the largest feeler gauge in the left coupler cutout and lie it across build plate. Make sure it's parallel with the front edge of the build plate (X axis). Measure up to the feeler gauge with the small mm ruler, stay close to the left edge (looking at it from the front) of the build plate, 3mm in from the front of the build plate backwards (along Y axis) until you are in line with the left Z coupling cutout (to the post it note or feeler gauge). That's the Y value here, 94.00mm.

    -29.5,94                       # This is the left Z motor (stepper_z)
    258.5,94                       # This is the right Z motor (stepper_z1)
####################################
points:                            # The points to probe with the CRTouch on build surface. There's 2 for the Ender 3 Pro because only 2 steppers exist.
                                   
                                   # To find the X position value for the left stepper we need to grab the right side's value first. Move the X gantry all the way to the right, note the X value (251) in the GUI or KlipperScreen, drop CRTouch pin, jog the Z axis down a bit but don't push the pin up, use the pin to measure against with a small mm ruler. Measure from edge of build plate to pin. In my case it's 27mm. Subtract 3mm for the buffer on the right edge of the build plate, (27 - 3 = 24). Take the total distance and minus out the buffer (251 - 3 = 248), (248) is the right stepper X value for the position below.

                                   # To find the X probe position value for the left stepper, home, then jog to 0,0. From our config we know that our CRTouch is negative 45mm away (left of the nozzle) from the nozzle (x_offset: -45.00). We can use this value along with 0,0 to come 27mm inwards from the 3mm buffer at the left edge of the build plate. Home all, jog to 0,0, this puts the nozzle at 0,0 (3mm inwards from the front and left edge of the build plate). Move the Y axis to 122.50 so we have some room to test with on the build plate. Nozzle's at 0,0. Move the X axis to the right 45mm. That will put the CRTouch on this invisible 3mm buffer line. Lower the Z axis by 5mm. BLTOUCH_DEBUG command=pin_down. Because we're moving the X axis to the right we need to add the X value on the KlpiierScreen of 45mm to the 27mm we got on the right side (45+27 = 72mm). 72 is the left stepper X probe position. This will make the CRTouch probe the left and right sides of the build plate evenly. If you bring the X axis to the right side at (248) and measure in from the right side edge (forget the buffer), you'll get (29mm) and if you bring it all the way to the left at (72) and do the same you'll get (29mm). Congrats it's now probing equally on both the left and right sides.
                                   
                                   # Y value is 122.50 which is half of the build plate (245). Y's max travel is 230. Y's min travel is -15. So, 230 minus negative 15 = 245. 245/2 = 122.50. The Z steppers are in line with each other so both use 122.50mm as seen below. Doing it this way will center the build plate under the probe and make sure probing takes place in the center of the plate so it doesn't flex.

    72,122.50                      # Left stepper x,y (stepper_z)
    248,122.50                     # Right stepper x,y (stepper_z1)
####################################
speed: 50                          # How fast in mm/sec are we moving between probing points after the Z axis is raised. This should probably be under the horizontal_move_z command below.
####################################
horizontal_move_z: 5               # How far up are we moving the Z axis between left and right probing. High enough that it doesn't drag the nozzle across the bed.
####################################
retries: 6                         # How many times to retry until the tolerance is correct.
####################################
retry_tolerance: 0.0025            # Needs to be within this Z axis tolerance or else retries happen.
