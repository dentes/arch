# Arch Linux
## System Overview

---
---
**Table of Contents**  

1. [Exectutive Overview](#1)
2. [Installing Arch Linux](#2)
3. [Configuring Core Packages](#3)
  
---
---

### <a name="1"></a>Executive Overview

My Arch Linux setup is characterized by these core packages:
* [xmonad](http://xmonad.org/)
* [xmobar](http://projects.haskell.org/xmobar/)
* [zsh](http://www.zsh.org/) + [oh-my-zsh](http://ohmyz.sh/)
* [urxvt](http://software.schmorp.de/pkg/rxvt-unicode.html)
* [vim](http://www.vim.org/)  
  
It is meant to be fast, minimal, lightweight, use few resources, and look cool ! 

---
### <a name="2"></a>Installing Arch Linux

Download and Transfer the latest Arch Linux ISO to your device via Mac Terminal:  
```
$ diskutil unmout /dev/sdX  
$ sudo dd if=/path_to_arch_.iso of=/dev/sdX ba=1M
```
Then, boot up into the iso image on the device and follow the installation guide:
  
1. Pre-Installation
2. Installation
3. Post-Installation

---
  
1 Pre-Installation  
1.1. Set the keyboard layout  
1.2. Connect to the Internet  
```
// If you use wifi setup internet with:
$ ip link  
$ wifi-menu   
$ ping -c 3 8.8.8.8  
$ ping -c 3 www.google.com
```

1.3. Update the system clock  
1.4. Partition the disks  
```$ parted /dev/sdX```

1.5. a. Format the partitions:  
```
$ (parted) mklabel msdos

// Set the range from 1MiB to however big you want
$ (parted) mkpart primary ext4 1MiB 10GiB  

// Set boot flag on it
$ (parted) set 1 boot on

// Set the range from end of last partition to the addition of your amount of RAM (or more)s  
$ (parted) mkpart primary linux-swap 10GiB 12GiB  
$ (parted) quit
```

1.5. b. Create File Systems (I chose ext4)  
```
$ mkfs.ext4 /dev/sdxN
$ mkswap /dev/sdXY
$ swapon /dev/sdXY
```

1.6 Mount the partitions
```$ mount /dev/sdXN /mnt``` 

---

2 Installation  
2.1 Select the mirrors
```$ nano /etc/pacman.d/mirrorlist```, by uncommenting the mirror nearest you  
2.2 Install the base packages
```$ pacstrap -i /mnt base base-devel```  
2.3 Configure the system:
```
// Generate your fstab file
$ genfstab -U  /mnt > /mnt/etc/fstab

// chroot in and setup timezone and clock
$ arch-chroot /mnt /bin/bash
$ nano /etc/locale.gen
$ locale-gen
$ echo LANG=en_US.UTF-8 > /etc/locale.conf
$ export LANG=en_US.UTF-8
$ tzselect
$ ln -s /usr/share/zoneinfo/America/New_York > /etc/localtime
$ hwclock --systohc --utc

// Set computer hostname
$ echo HOSTNAME > /etc/hostname

// Set root user password
$ passwd

// If you plan to use wifi, get this package now
$ pacman -S iw wpa_supplicant dialog
```

2.4 Install a boot loader (I use grub here).  
Note: if you plan to dual-boot another OS, get the os-prober package as well!
```         
$ pacman -S grub os-prober
$ grub-install --recheck --target=i386-pc /dev/sdX
$ grub-mkconfig -o /boot/grub/grub.cfg

// Exit out of root user
$ exit
```

2.5 Reboot
```
$ umount -R /mnt
$ reboot  
```

---

3 Post-Installation  
First, login as the root user
```
$ ip link
$ systemctl stop dhcpcd@enXXX.service        // disable DCHP if on wifi
$ systemctl enable dhcpcd@enXXX.service      // or enable DCHP by default
$ systemctl start dhcpcd@enXXX.service
```

To add user(s):
```
$ useradd -m -G wheel -s /bin/bash USERNAME
$ passwd USERNAME
```

Uncomment the line: `%wheel ALL=(ALL) ALL` after running:
```
$ EDITOR=nano visudo            
```

Some essential packages:
```
// Get the sudo package, as well as bash auto-completion
$ pacman -S sudo
$ pacman -S bash-completion

// If on a laptop, get:
$ pacman -S xf86-input-synaptics
```

Third, run `$ nano /etc/pacman.conf` and uncomment the following lines if on a 64-bit machine:
```
[multilib]
Include = /etc/pacman.d/mirrorlist  
```

In the same file, add this at the bottom to use the AUR repositories:
```
[archlinuxfr]
SigLevel = Never
Server = [http://repo.archlinux.fr/$arch](http://repo.archlinux.fr/$arch)
```

Update repositories and grab the yaourt package: 
```
$ pacman -Sy
$ pacman -S yaourt
```
   
Installing a DE
```
$ pacman -S xorg-server xorg-server-utils

// For Gnome 3
$ pacman -S gnome
$ $ systemctl start gdm.service
$ $ systemctl enable gdm.service

// For KDE Plasma 5
$ pacman -S plasma
$ pacman -S sddm
$ systemctl start sddm.service
$ systemctl enable sddm.service

// For Xfce4
$ pacman -S xfce4
$ startxs
```

// Choose GPU driver, depending on your machine
```
$ pacman -S xf86-video-intel
$ pacman -S nvidia nvidia-libgl
$ pacman -S xf86-video-ati lib32-mesa-libgl
```

Installing s WM: xmonad
```
$ pacman -S xmonad
```

4 Some useful Arch commands:

        $ sudo pacman -Sy      // To update the repositories
        $ sudo pacman -Syu     // To update system
        $ sudo pacman -Rns     // To remove any package
        $ yaourt -Syua         // To update packages from AUR

---

### <a name="3"></a>Configuring Core Packages

1. [xmonad]()
2. [xmobar]()
3. [zsh + oh-my-zsh]()
4. [urxvt]()
5. [vim]()
  
---

