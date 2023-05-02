
TP4 : Real Service
Partie 1 : Partitionnement du serveur de stockage
Partitionner le disque à l'aide de LVM
PV
[bossautruche@storagetp4 ~]$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sr0          11:0    1 1024M  0 rom  
vda         252:0    0    8G  0 disk 
├─vda1      252:1    0  600M  0 part /boot/efi
├─vda2      252:2    0    1G  0 part /boot
└─vda3      252:3    0  6.4G  0 part 
  ├─rl-root 253:0    0  5.6G  0 lvm  /
  └─rl-swap 253:1    0  820M  0 lvm  [SWAP]
vdb         252:16   0    2G  0 disk 
VG
[bossautruche@storagetp4 ~]$ sudo pvcreate /dev/vdb
  Physical volume "/dev/vdb" successfully created.

[bossautruche@storagetp4 ~]$   sudo vgcreate storage /dev/sdb
  Volume group "storage" successfully created

[bossautruche@storagetp4 ~]$ sudo lvcreate -l 100%FREE storage --name truc2oof
  Logical volume "truc2oof" created

[bossautruche@storagetp4 ~]$ sudo mkfs -t ext4 /dev/storage/tp4_data
mke2fs 1.46.5 (30-Dec-2021)
Discarding device blocks: done                            
Creating filesystem with 523264 4k blocks and 130816 inodes
Filesystem UUID: db1f4729-9809-4404-9d37-7e2c7dc3d304
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done

[bossautruche@storagetp4 ~]$ sudo mkdir /storage
[bossautruche@storagetp4 ~]$ sudo mount /dev/storage/tp4_data /storage/
[bossautruche@storagetp4 ~]$ sudo vim /etc/fstab  
[bossautruche@storagetp4 ~]$ sudo umount /storage/ 

[bossautruche@storagetp4 ~]$ sudo dnf install -y nfs-utils 

[bossautruche@storagetp4 ~]$ sudo mkdir -p /storage/site_web_{1,2}
[bossautruche@storagetp4 ~]$ sudo chown -R nobody /storage/site_web_*








