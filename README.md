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

# Generate file system tables 
genfstab -U /mnt >> /mnt/etc/fstab

# Chroot into the new installation
arch-chroot /mnt

#--------Changes "root@archiso~#" to "[root@archiso /]#"

# Troubleshoot the timedatectl list-timezones error 
Further research needed 

Error:
"System has not been booted with systems as init system (PID 1).
Failed to connect to bus: Host is down"

# Sync the system & hardware clocks 
hwclock --systohc

# Select the correct locale
nano /etc/locale.gen

#--------Unhash the en_US.UTF-8 UTF-8 locale

#--------Save & Exit, then generate the locales with:

locale-gen

#--------Edit the locale.conf 

nano /etc/locale.conf

#-------Add "LANG=en_US.UTF-8" save&exit

# Give a name to the host machine 
echo "Name" >> /etc/hostname

# Edit the hosts file 
nano /etc/hosts

#-------- Add in the following

127.0.0.1  localhost

::1        localhost

127.0.1.1  "hostname".localdomain    "hostname"

# Give a pass to the root user
passwd

# Package installation
pacman -S grub efibootmgr networkmanager network-manager-applet
dialog mtools dosfstools wpa_supplicant git bash-completion
base-devel linux-headers bluez bluez-utils inetutils
dnsutils gvfs gvfs-smb openssh nvidia nvidia-utils 

# Add the GPU card module 
nano /etc/mkinitcpio.conf 

#In the MODULES=() add nvidia
MODULES=(nvidia)

#Then regenerate the file with the Linux parameter or linux-lts

mkinitcpio -p linux

# GRUB installation 
grub-instal --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB

#Generate the grub config file

grub-mkconfig -o /boot/grub/grub.cfg

# Enable Services
systemctl enable NetworkManager

systemctl enable Bluetooth

systemctl enable fstrim.trim

# Add a user
useradd -mG wheel "name"

passwd "name"

#Uncomment the first wheel group

%wheel ALL=(ALL) ALL

# exit, umount -a, reboot 

