# Arch BSPWM clean install personal notes

# WiFi Configuration
ip a

#--------if WiFi state is down

iwctl

station wlan0 connect "Name of network"

"Password"

exit

# Sync Pacman 
pacman -Syy


# Formatting of the disk & partition creation
#--------Start off by checking the current layout

lsblk


#--------Open & wipe the disk in its entirety

gdisk /dev/"diskname"
   

#--------Delete all partitions on the disk

o  to wipe all

Y  to proceed

w  to write changes 

Y  to proceed 


#--------Partition Creation


n  to create new 

1st.  500M ef00 #EFI Partition 

2nd.  4G.  8200 #SWAP Partition

3rd.  50G  blank #ROOT Partition

4th.  Rest blank #HOME Partition

# Formatting of the partitions 

#-------Formating of the EFI partition

mkfs.fat -F32 /dev/"1st"

#--------Formating & activation of SWAP partition

mkswap /dev/"2nd"

swapon /dev/"2nd"

#--------Formating of ROOT partition

mkfs.ext4 /dev/"3rd"

#--------Formatting of HOME partition

mkfs.ext4 /dev/"4th"

# Mounting of the created partitions
#-------- This is the installation directory

mount /dev/"3rd" /mnt

#-------- Mounting the BOOT(EFI) partition

mkdir -p /mnt/boot/efi

mount /dev/"1st" /mnt/boot/efi

#-------- Mounting the HOME partition

mkdir /mnt/home 

mount /dev/"4th" /mnt/home

#-------- Check the partitions 

lsblk

# Installation of the base packages
pacstrap /mnt base linux linux-firmware vim nano intel-ucode


