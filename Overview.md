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

Download and Transfer the latest Arch Linux ISO to your device with:  
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
$ ip link  
$ wifi-menu   
$ ping -c 3 8.8.8.8  
$ ping -c 3 www.google.com
```

1.3. Update the system clock  
1.4. Partition the disks  
```
$ parted /dev/sdx
```

1.5. a. Format the partitions  
```
$ (parted) mklabel msdos  
$ (parted) mkpart primary ext4 1MiB 10GiB  
$ (parted) set 1 boot on  
$ (parted) mkpart primary linux-swap 10GiB 12GiB  
$ (parted) quit
```

1.5. b. Create File Systems  
```
$ mkfs.ext4 /dev/sdxN
$ mkswap /dev/sdXY
$ swapon /dev/sdXY
```

1.6 Mount the partitions
```
$ mount /dev/sdXN /mnt
```

---

2 Installation  
2.1 Select the mirrors
          ```$ nano /etc/pacman.d/mirrorlist```

2.2 Install the base packages
          ```$ pacstrap -i /mnt base base-devel```  

2.3 Configure the system
```
$ genfstab -U  /mnt > /mnt/etc/fstab
$ arch-chroot /mnt /bin/bash
$ nano /etc/locale.gen
$ locale-gen
$ echo LANG=en_US.UTF-8 > /etc/locale.conf
$ export LANG=en_US.UTF-8
$ tzselect
$ ln -s /usr/share/zoneinfo/America/New_York > /etc/localtime
$ hwclock --systohc --utc
$ echo HOSTNAME > /etc/hostname
$ passwd
$ pacman -S iw wpa_supplicant dialog
```

2.4 Install a boot loader
```         
$ pacman -S grub os-prober      // os-prober for dual-booting
$ grub-install --recheck --target=i386-pc /dev/sdX
$ grub-mkconfig -o /boot/grub/grub.cfg
$ 
$ 
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
$ systemctl stop dhcpcd@enXXX.service        // disable off DCHP if wifi
$ systemctl enable dhcpcd@enXXX.service      // enable DCHP by default
$ systemctl start dhcpcd@enXXX.service
$ useradd -m -G wheel -s /bin/bash USERNAME
$ passwd USERNAME
$ pacman -S sudo
```

Second, uncomment the line: `%wheel ALL=(ALL) ALL`. Then run the following:
```
$ EDITOR=nano visudo            
$ pacman -S bash-completion
```

Third, run `$ nano /etc/pacman.conf` and uncomment the following lines:
```
[multilib]
Include = /etc/pacman.d/mirrorlist  
```

In the same file, to use the AUR repositories, add at the bottom:
```
[archlinuxfr]
SigLevel = Never
Server = [http://repo.archlinux.fr/$arch](http://repo.archlinux.fr/$arch)
```


          
          
          $ pacman -Sy
          $ pacman -S xf86-input-synaptics
          $ pacman -S yaourt
          $ 
          $ 
    // If you want a DE
          $ pacman -S xorg-server xorg-server-utils

          $ pacman -S gnome
          + $ systemctl start gdm.service
          + $ systemctl enable gdm.service

          $ pacman -S plasma
          $ pacman -S sddm
          $ systemctl start sddm.service
          $ systemctl enable sddm.service

          // Choose GPU driver
          $ pacman -S xf86-video-intel
          $ pacman -S nvidia nvidia-libgl
          $ pacman -S xf86-video-ati lib32-mesa-libgl

    // XMONAD
          $ pacman -S xmonad
          $ 
          $ 


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

