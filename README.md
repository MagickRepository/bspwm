# Arch BSPWM clean install personal notes

# WiFi Configuration
ip a

   #if WiFi state is down

iwctl

station wlan0 connect "Name of network"

"Password"

exit

# Sync Pacman 
pacman -Syy


# Formatting of the disk & partition creation
   #Start off by checking the current layout

lsblk


   #Open & wipe the disk in its entirety

gdisk /dev/"diskname"
   

   #Delete all partitions on the disk

o  to wipe all

Y  to proceed

w  to write changes 

Y  to proceed 


#Partition Creation


n  to create new 
##

1st.  500M ef00 #EFI Partition 

2nd.  4G.  8200 #SWAP Partition

3rd.  50G  blank #

4th.  Rest blank #
