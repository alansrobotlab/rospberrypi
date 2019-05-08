___
# ROSpberryPi

#### A quick and dirty way of bootstrapping ROS and some additional tools onto a Raspberry Pi Zero / W.  (well, maybe not quick, it still takes more than an hour to complete the script and get everything installed)
___
## FAQ:
#### Why?
- Recent ROS distributions only support ROS on the Raspberry pi 2B or greater (all arm7l devices) and on Ubuntu Mate.  The Raspberry Pi Zero/W is an arm6l device, so you if you want ROS on it you must compile it yourself which is a bit of a pain.  So I put together all of my work in a separate repository so others can take advantage of a prebuilt ROS Melodic setup.
- You can pick up a Pi Zero / W for a little as $5, which opens up all kinds of options for small projects
- If you operate the Pi Zero as headless you can do all of your development work on another machine, and just push the code to the zero.  My current setup has:
    - the pi zero w set up as an access point (using create_ap)
    - connect to it from my laptop using ssh/putty.  
    - the ros src directory is shared and you can connect to it from \\\\(whatever your pi zero w ip address is)\\ros
    - open the shared folder in visual studio code
#### How?
 - I downloaded and configured all of the required sources and compiled them on a Raspberry Pi 3 (I wouldn't recommend trying to compile them directly on a zero).  Next I packaged up the installs using checkinstall and hosted them on my google drive.  Then I wrote a script which grabs everything necessary for a functional ROS Melodic install.
#### What works?
 - Almost everything!  (As far as I know)
#### What doesn't work?
 - rviz can't seem to load urdf/stl files without crashing (but you shouldn't be running these tools on the zero anyways)
 - visual studio code will not work on a pi zero (it relies on electron, which only compiles for the arm7l, and the pi zero is an arm6l)
#### Will this work on any Raspberry Pi?
 - In theory this should work on any Raspian Stretch install from the zero and original A+ to the 3B+.
#### Will this work on a Pi 2B/3B/3B+?
 - In theory, but for those devices it would be much easier to just install ubuntu mate and use the official packages
___
## Requirements:
 - A Raspberry Pi Zero (preferably a W)
 - Raspbian Stretch (Desktop or Minimal)
 - An Internet connection
___
## What this script installs:
 - A compatible version of TinyXML
 - A compatible precompiled arm6l version of OpenCV3
 - A compatible precompiled arm6l version of 
     - ROS Melodic Desktop
     - ROS Melodic Perception (minus the pcl stuff)
     - ROS Melodic Robot
     - ROS Melodic Joy(stick)
     - All Dependencies
 - Tensorflow
 - Increases Swap to 1GB
 - GPIZero libraries
 - Installs create_ap (ability to use raspi zero w as access point)
 - Initializes a ROS workspace
  - Sets up a SAMBA Fileshare for the /home/pi/ros/src directory
 - Sets up all env variables for ROS melodic and your ros workspace
___
## How to set this up
1. Download and install Raspbian Stretch (Desktop or Lite) onto a microsd card
2. Log into the pi and set it up to access the internet
    - with full desktop, just use the UI
        https://www.raspberrypi.org/documentation/configuration/wireless/desktop.md
    - with lite, using raspi-config
        https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md
    - with lite, by configuring two files in the /boot directory
        https://www.raspberrypi.org/documentation/configuration/wireless/headless.md
    - with lite, using a UART (going headless)
        https://learn.adafruit.com/raspberry-pi-zero-creation
3. If you're feeling adventurous, maybe overclock the zero a bit (I'm not recommending you do this and I'm not going to walk you through how -- but possible settings are)
   ```sh
    # https://www.raspberrypi.org/forums/viewtopic.php?t=177743#p1169042
    # 10% overclock
    arm_freq=1100
    over_voltage=8
    sdram_freq=500
    sdram_over_voltage=2
    force_turbo=1
    boot_delay=1
    dtparam=sd_overclock=100
    ```
4. Open up a command prompt and type in the following commands.
    ```sh
    git clone https://github.com/alansrobotlab/rosberrypizero
    cd rosberrypizero
    ```
5.  Read through the install script so you understand what you're about to do. (You're not the type of person that runs random scripts you download from the internet are you?)
    ```sh
    nano bootstrap.sh
    ```
6. Run the bootstrap script and go get a cup of coffee.
   ```sh
   sh bootstrap.sh
   ```
7. Review the results of the bootstrap script, looking for any errors
8. Carefully read the last lines of the bootstrap script.
    ```
    OK, now there are a few more things that you need to do
    before you can get started.
    First, run the following command in a command prompt
    to set the samba filesharing password
    for user 'pi'\n (Must be at least 8 characters)
    
    sudo smbpasswd -a pi
    
    
    Next, disconnect from your wifi network and run
    the following command to set up your pi as an access point
    
    sudo iw wlan0 disconnect (to disconnect wifi)
    sudo systemctl enable create_ap (set access point to begin automatically on startup)
    sudo systemctl start create_ap (to start the create_ap access point)
    
    
    Finally, run the following command to enable ssh, the camera and i2c
    
    sudo rc-gui (for the gui config tool) \n\
    sudo raspi-config (for the command line tool) \n"
   ```
9.  http://wiki.ros.org/ROS/Tutorials
10. If you ever want to grab an updated version of this repository
    ```sh
    cd ~/.rosberrypizero # whereever you cloned this repository
    git pull
    ```
___
