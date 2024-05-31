#  <sup>Arch-Linux-Install-T470s </sup> <img src="https://res.cloudinary.com/practicaldev/image/fetch/s--ndNn_V3d--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/i/tuhli2hqgo0h8723vd51.png" width="60">

### Step 1: Connect to internet / Sync Time
```
iwctl --passphrase "PASSWORD" station wlan0 connect "WIFI-NETWORK NAME"

timedatectl set-ntp true
```

### Step 2: Partition Disk
```
cfdisk /dev/nvme0n1

Delete all existing partions and create 3 partions as follows:

1) 1GB (/dev/efi_system_partition) - (EFI System Partition)

2) 5GB (/dev/swap_partition) - (Linux Swap)

3) Remaining Storage (/dev/root_partition) - (Linux x86-64 root (/))

Write: YES
```

### Step 3: Format the partitions
```
mkfs.fat -F 32 /dev/nvme0n1p1

mkswap /dev/nvme0n1p2

mkfs.ext4 /dev/nvme0n1p3
```

### Step 4: Mount the file systems
```
swapon /dev/nvme0n1p2

mount /dev/nvme0n1p3 /mnt
```

### Step 5: Install Essential Packages
```
pacstrap -K /mnt base linux linux-firmware
```

### Step 6: Fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```

### Step 7: Chroot
```
arch-chroot /mnt
```

### Step 8: Time
```
ln -sf /usr/share/zoneinfo/"REGION"/"CITY" /etc/localtime

hwclock --systohc
```

### Step 9: Install required packages
```
pacman -S nano sudo networkmanager network-manager-applet grub dosfstools linux-headers intel-ucode efibootmgr reflector
```

### Step 10: Localization
```
nano /etc/locale.gen (Uncomment "en_US.UTF-8 UTF-8")

locale-gen

nano /etc/locale.conf

type "LANG=en_US.UTF-8"
```

### Step 10: Network Configuration
```
nano /etc/hostname (Type in desired name)

nano /etc/hosts
  127.0.0.1        localhost
  ::1              localhost
  127.0.1.1        "HOSTNAME"

systemctl enable NetworkManager
```

### Step 11: Root Password
```
passwd
```

### Step 12: Create User w/ Password and Enable Sudo Privelleges
```
useradd -m -G wheel "NAME"

passwd "NAME"

nano /etc/sudoers
  Uncomment "%wheel ALL=(ALL) ALL"
```

### Step 13: Mount EFI File Partition
```
mkdir /boot/efi

mount /dev/nvme0n1p1 /boot/efi
```

### Step 14: Install and Configure Grub
```
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

grub-mkconfig -o /boot/grub/grub.cfg
```

### Step 15: Install Gnome and Enable It
```
pacman -S gnome

systemctl enable gdm
```

### Step 16: Finalize and Boot into system
```
exit

umount -R /mnt

reboot
```
