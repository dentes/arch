# Arch Linux
## System Overview

---
**Table of Contents**  
  0. [Exectutive Overview](#0)  
  1. [Installing Arch Linux](#1)  

### <a name="0"></a>Executive Overview

My Arch Linux setup is characterized by these packages:
* xmonad
* xmobar
* zsh + oh-my-zsh
* urxvt
* vim     
  
It is meant to be fast, minimal, lightweight, use few resources, and look cool ! 

---
### <a name="1"></a>Installing Arch Linux

  0. Download & Transfer ISO
  1. Pre-Installation
  2. Installation
  3. Post-Installation

---
  
**0. Download & Transfer ISO***  
    0.1. Download the latest .iso  
    0.2. Connect destination device  
    0.3. Transfer .iso to destination  
``
        $ sudo dd if=/path_to_arch_.iso of=/dev/sdX ba=1M
``
0.4. Boot new device  
  
**1. Pre-Installation**  
1.1. Set the keyboard layout  
1.2. Connect to the Internet  

    $ ip link  
    $ wifi-menu   
    $ ping -c 3 8.8.8.8  
    $ ping -c 3 www.google.com

1.3. Update the system clock  
1.4. Partition the disks 

    $ parted /dev/sdx
   
1.5. Format the partitions  

    $ (parted) mklabel msdos  
    $ mkpart primary ext4 1MiB 10GiB  
    $ set 1 boot on  
    $ mkpart primary linux-swap 10GiB 12GiB  
    $ quit  

// Create File Systems  

          $ mkfs.ext4 /dev/sdxN
          $ mkswap /dev/sdXY
          $ swapon /dev/sdXY

1.6. Mount the partitions

          $ mount /dev/sdXN /mnt  

---

**2. Installation**
    2.1 Select the mirrors
          $ nano /etc/pacman.d/mirrorlist
    2.2 Install the base packages
          $ pacstrap -i /mnt base base-devel   
    2.3 Configure the system
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
    2.4 Install a boot loader
          $ pacman -S grub os-prober      // os-prober for dual-booting
          $ grub-install --recheck --target=i386-pc /dev/sdX
          $ grub-mkconfig -o /boot/grub/grub.cfg
          $ 
          $ 
          $ exit
    2.5 Reboot
          $ umount -R /mnt
          $ reboot  

---

**3. Post-Installation**
        >> login as root
          $ ip link
          $ systemctl stop dhcpcd@enXXX.service        // disable off DCHP if wifi
          $ systemctl enable dhcpcd@enXXX.service      // enable DCHP by default
          $ systemctl start dhcpcd@enXXX.service
          $ useradd -m -G wheel -s /bin/bash USERNAME
          $ passwd USERNAME
          $ pacman -S sudo
          $ EDITOR=nano visudo            // UNCOMMENT: %wheel ALL=(ALL) ALL
          $ pacman -S bash-completion
          $ nano /etc/pacman.conf         // UNCOMMENT: [multilib]
                                                        Include = /etc/pacman.d/mirrorlist  
                                          // ADD:       [archlinuxfr]
                                                        SigLevel = Never
                                                        Server = [http://repo.archlinux.fr/$arch](http://repo.archlinux.fr/$arch)
          $ pacman -Sy
          $ pacman -S xf86-input-synaptics
          $ paceman -S yaourt
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


**4. Some useful Arch commands:**  

          $ sudo pacman -Sy      // To update the repositories
          $ sudo pacman -Syu     // To update system
          $ sudo pacman -Rns     // To remove any package
          $ yaourt -Syua         // To update packages from AUR



