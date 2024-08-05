### RAID 1 with 2 disks

```
# Create RAID partitions
sudo fdisk /dev/sdb
sudo fdisk /dev/sdc

# Create RAID 1
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1

# Check RAID status
cat /proc/mdstat

# Format RAID
sudo mkfs.ext4 /dev/md0

# Mount RAID
sudo mkdir /mnt/raid1
sudo mount /dev/md0 /mnt/raid1
```

### Fake a failure and rebuild RAID 1

```
# Fake a failure on disk 1
sudo mdadm /dev/md0 --fail /dev/sdb1

# Check status
cat /proc/mdstat

# Remove compromised disk
sudo mdadm /dev/md0 --remove /dev/sdb1

# Add a new disk
sudo mdadm /dev/md0 --add /dev/sdb1

# Check rebuild
cat /proc/mdstat
```

### RAID 5 with 3 disks

```
# Delete existing RAID 1
# sudo umount /mnt/raid1
# sudo mdadm --stop /dev/md0
# sudo mdadm --zero-superblock /dev/sdb1 /dev/sdc1

# Create partions on 3 disks
sudo fdisk /dev/sdb
sudo fdisk /dev/sdc
sudo fdisk /dev/sdd

# Create RAID 5
sudo mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb1 /dev/sdc1 /dev/sdd1

# Check RAID status
cat /proc/mdstat

# Format RAID
sudo mkfs.ext4 /dev/md0

# Mount RAID
sudo mkdir /mnt/raid5
sudo mount /dev/md0 /mnt/raid5
```
