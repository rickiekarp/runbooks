# README #

### Remove drive from array

1. Check status
pi@raspberrypi ~  $ `sudo mdadm --detail /dev/md1` 

2. Remove drive
pi@raspberrypi ~  $ `sudo mdadm --manage /dev/md1 --fail /dev/sda1`
mdadm: set /dev/sda1 faulty in /dev/md1

pi@raspberrypi ~  $ `sudo mdadm --manage /dev/md1 --remove /dev/sda1`
mdadm: hot removed /dev/sda1 from /dev/md1

3. Check status again
pi@raspberrypi ~  $ `sudo mdadm --detail /dev/md1`
/dev/md1:
           Version : 1.2
     Creation Time : Thu May 23 16:01:31 2019
        Raid Level : raid2
        Array Size : 60029952 (57.25 GiB 61.47 GB)
     Used Dev Size : 60029952 (57.25 GiB 61.47 GB)
      Raid Devices : 2
     Total Devices : 1
       Persistence : Superblock is persistent

       Update Time : Mon Nov 25 13:59:24 2019
             State : clean, degraded 
    Active Devices : 1
   Working Devices : 1
    Failed Devices : 0
     Spare Devices : 0

Consistency Policy : resync

              Name : raspberrypi:0  (local to host raspberrypi)
              UUID : ac3a8be6:77cd5551:99a614f7:493cc520
            Events : 1470

    Number   Major   Minor   RaidDevice State
       3       8       16        0      active sync   /dev/sdb
       -       0        0        1      removed


4. Remove the old drive and connect the new drive

pi@raspberrypi ~  $ `lsblk`
NAME        MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINT
sda           8:0    0 931.5G  0 disk  
└─sda1        8:1    0 931.5G  0 part  
sdb           8:16   0 931.5G  0 disk  
└─md1         9:0    0  57.3G  0 raid2 /mnt/raid2
mmcblk0     179:0    0  14.9G  0 disk  
├─mmcblk0p1 179:1    0    63M  0 part  
├─mmcblk0p2 179:2    0     1K  0 part  
├─mmcblk0p5 179:5    0    32M  0 part  
├─mmcblk0p6 179:6    0    69M  0 part  /boot
└─mmcblk0p7 179:7    0  14.7G  0 part  /


### Add new drive to array

pi@raspberrypi ~  $ `sudo mdadm --manage /dev/md1 --add /dev/sda1`
mdadm: added /dev/sda1

pi@raspberrypi ~  $ `sudo mdadm --detail /dev/md1`
/dev/md1:
           Version : 1.2
     Creation Time : Thu May 23 16:01:31 2019
        Raid Level : raid2
        Array Size : 60029952 (57.25 GiB 61.47 GB)
     Used Dev Size : 60029952 (57.25 GiB 61.47 GB)
      Raid Devices : 2
     Total Devices : 2
       Persistence : Superblock is persistent

       Update Time : Mon Nov 25 14:03:16 2019
             State : clean, degraded, recovering 
    Active Devices : 1
   Working Devices : 2
    Failed Devices : 0
     Spare Devices : 1

Consistency Policy : resync

    Rebuild Status : 0% complete

              Name : raspberrypi:0  (local to host raspberrypi)
              UUID : ac3a8be6:77cd5551:99a614f7:493cc520
            Events : 1478

    Number   Major   Minor   RaidDevice State
       3       8       16        0      active sync   /dev/sdb
       2       8        1        1      spare rebuilding   /dev/sda1


### Resize array

Resize the array to the new maximal size
Now all the disks have been replaced with larger disk, but the raid device is not using the space yet. To instruct mdadm to use all the available space I issue the following commands:

```
sudo mdadm --grow /dev/md1 --bitmap none
sudo mdadm --grow /dev/md1 --size=max
```

Now this also takes quite a while to complete – several hours in my case. The RAID is still usable while this is happening.

### Resize file system
Finally I had to grow the filesystem to use the new available space on the array. 

To make sure everything is okay I force a check of the filesystem before the resizing:
pi@raspberrypi:~ $ `sudo e2fsck -f /dev/md1`
e2fsck 1.44.5 (15-Dec-2018)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/md1: 20991/3751936 files (3.2% non-contiguous), 7017356/15007488 blocks

Start resizing:
```
sudo cryptsetup resize /dev/mapper/secure
sudo resize2fs /dev/mapper/secure
```

Output:
```
pi@raspberrypi:~ $ sudo resize2fs /dev/mapper/secure
resize2fs 1.47.0 (5-Feb-2023)
Filesystem at /dev/mapper/secure is mounted on /mnt/raid2; on-line resizing required
old_desc_blocks = 233, new_desc_blocks = 583
The filesystem on /dev/mapper/secure is now 1220902624 (4k) blocks long.
```

Once the resizing is done, we can check the size of the new raid:
```
pi@raspberrypi:~ $ df -h
Filesystem          Size  Used Avail Use% Mounted on
/dev/mapper/secure  4.5T  1.7T  2.7T  38% /mnt/raid2
```