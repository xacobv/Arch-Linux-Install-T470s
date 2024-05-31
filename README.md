#  <sup>Arch-Linux-Install-T470s </sup> <img src="https://res.cloudinary.com/practicaldev/image/fetch/s--ndNn_V3d--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/i/tuhli2hqgo0h8723vd51.png" width="60">

### Step 1: Connect to internet
```
iwctl --passphrase "PASSWORD" station wlan0 connect "WIFI-NETWORK NAME"
```

### Step 2: Partition Disk
```
cfdisk /dev/nvme0n1

Delete all partions and create 3 partions as follows:

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

### Step 4: Mount the file system
```
mount --mkdir /dev/nvme0n1p1 /mnt/boot

swapon /dev/nvme0n1p2

mount /dev/nvme0n1p3/mnt
```

### Step 5: Install Essential Packages
```
pacstrap -K /mnt base linux linux-firmware
pacstrap /mnt nano networkmanager
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

### Step 9: Localization
```
locale-gen


```
