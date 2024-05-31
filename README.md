#  <sup>Arch-Linux-Install-T470s </sup> <img src="https://res.cloudinary.com/practicaldev/image/fetch/s--ndNn_V3d--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/i/tuhli2hqgo0h8723vd51.png" width="60">

### Step 1: Connect to internet
```
iwctl --passphrase "PASSWORD" station wlan0 connect "WIFI-NETWORK NAME"
```

### Step 2: Partition Disks
```
cfdisk /dev/nvme0n1

Delete all partions and create 3 partions as follows:

1) 1GB (/dev/efi_system_partition) - (EFI System Partition)

2) 5GB (/dev/swap_partition) - (Linux Swap)

3) Remaining Storage (/dev/root_partition) - (Linux x86-64 root (/))

Write: YES
```

### Step 3: 
