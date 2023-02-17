#--------Arch BSPWM clean install--------

# check for the WiFi station name
ip a
    
# WiFi station name is usually wlan0

# if WiFi state is down:
iwctl
station wlan0 connect "Name of network"
"Password"
exit

#-----------------------------------------
# Sync Pacman 
pacman -Syy

#-----------------------------------------
# Formatting of the disk & partition creation
  #1. Start off by checking the current layout
   lsblk

  #2. Open & wipe the disk in its entirety
   gdisk /dev/"diskname"
   
  #3. Delete all partitions on the disk
   3.1 o    
   3.2 Y to proceed
   3.3 w to write changes 
   3.4 Y to proceed 

  #4. Partition Creation
   # n to create new 
   4.1 500M ef00 #EFI Partition 
   4.2 4G.  8200 #SWAP Partition
   4.3 50G  blank #
   4.4 Rest blank #
