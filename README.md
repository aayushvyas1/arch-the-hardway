# arch-the-hardway
install by using traditional method. 


### Using iWCTL for WIFI Connection

If using built-in WiFi support,  use the iwctl tool to connect to the internet.

To find list of network interfaces type: iwctl device list

To search for WIFI Networks type: iwctl station wlan0 get-networks

To connect to WIFI network type: iwctl station wlan0 connect WIFI_NAME

Once connected run : ping google.com to check internet connection (Interrupt Ping with CTRL + C)

Then type the command to syncnorize the package database: pacman -Sy

Then Type this command : pacman -S archlinux-keyring



### Create Partitions For Arch Linux

Type lsblk to list all connected drives on the PC

Type cfdisk /dev/nvme0n1 and press enter

Scroll to free space and create 3 partitions: EFI, ROOT, and swap



### Formatting Partitions 

Format the Arch EFI Partition (e.g., NVME0N1p5)
mkfs.fat -F32 /dev/nvme0n1p5

Format the root partition (e.g., NVME0N1p6)
mkfs.ext4 /dev/NVME0N1p6

Format the swap partition (e.g., nvme0n1p7)
mkswap /dev/nvme0n1p7


### Mount Partitions

mkdir /mnt/boot

mount /dev/nvme0n1p6 /mnt

mount /dev/nvme0n1p5 /mnt/boot

swapon /dev/nvme0n1p7


### Install

pacstrap -i /mnt base base-devel linux linux-headers linux-firmware amd-ucode sudo git nano cmake make bluez bluez-utils networkmanager cargo gcc


### Generate File System Table 

genfstab -U /mnt >> /mnt/etc/fstab


### CHROOT To Newley Installed system

 arch-chroot /mnt

 Change Root Password

 Then type passwd and set the root password. This is a superuser account and is known as a top-tier user in Linux. 

 ### Adding a Standard User

 useradd -m -g users -G wheel,storage,video,audio -s /bin/bash USER_NAME

passwd USER_NAME

visudo

Uncomment the line in the image and save the changes with CTRL + O and CTRL + X to Exit


### SetupTimezone/Region

ln -sf /usr/share/zoneinfo/ 

ln -sf /usr/share/zoneinfo/Asia/Kolkata /etc/localtime

Synchronize the current time with the hardware clock using `hwclock --systohc`

Language - `nano /etc/locale.gen`

`locale-gen`

`echo "LANG=en_US.UTF-8" >> /etc/locale.conf`


### Host 

echo "archlinux" >> /etc/hostname

/etc/hosts


### Grub 

pacman -S grub efibootmgr dosfstools mtools

grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

grub-mkconfig -o /boot/grub/grub.cfg

### services

systemctl enable bluetooth

systemctl enable NetworkManager

exit 

umount -lR /mnt

reboot

### after install

connect to internet again

Install gui 

sudo pacman -S xorg sddm plasma-meta plasma-wayland-session kde-applications noto-fonts noto-font-emoji ttf-dejavu ttf-font-awesome

sudo systemctl enable sddm

sudo pacman -Sy flatpak

sudo pacman -Sy nvidia


### adding win to grub 


Install OS-Prober with the command provided
    sudo pacman -Sy os-prober
Edit the grub file, sudo nano /etc/default/grub
    Set the GRUB_TIMEOUT=20
Scroll to the bottom and uncomment the line related to os-prober.

grub-mkconfig -o /boot/grub/grub.cfg 


