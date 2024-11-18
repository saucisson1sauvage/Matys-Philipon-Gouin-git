### 🌞 Lister les périphériques de stockage branchés à la machine

```powershell
aa@vbox:~$ lsblk
NAME           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda              8:0    0   20G  0 disk
└─sda1           8:1    0   20G  0 part
   ├─zizi-root  254:0    0  9.3G  0 lvm  /
   ├─zizi-home  254:1    0  1.9G  0 lvm  [SWAP]
   ├─zizi-var   254:2    0  2.8G  0 lvm  /var
   └─zizi-swap  254:3    0  1.9G  0 lvm  /home
sr0             11:0    1 1024M  0 rom
```

### 🌞 Lister les partitions en cours d'utilisation
```powershell
aa@vbox:~$ df -h
Filesystem               Size  Used Avail Use% Mounted on
udev                    961M     0  961M   0% /dev
tmpfs                   198M  1.2M  196M   1% /run
/dev/mapper/zizi-root   9.1G  3.7G  4.9G  43% /
tmpfs                   986M     0  986M   0% /dev/shm
tmpfs                   5.0M  4.0K  5.0M   1% /run/lock
/dev/mapper/zizi-swap   1.8G   18M  1.7G   2% /home
/dev/mapper/zizi-var    2.7G  420M  2.2G  17% /var
tmpfs                   198M  112K  197M   1% /run/user/1000
```

### 🌞 Lister la quantité d'inodes disponibles sur chaque partition

```powershell
aa@vbox:~$ df -i
Filesystem            Inodes  IUsed  IFree IUse% Mounted on
udev                  245784    397 245387    1% /dev
tmpfs                 252303    700 251603    1% /run
/dev/mapper/zizi-root 610800 140089 470711   23% /
tmpfs                 252303      1 252302    1% /dev/shm
tmpfs                 252303      4 252299    1% /run/lock
/dev/mapper/zizi-swap 121920    416 121504    1% /home
/dev/mapper/zizi-var  183264  11025 172239    7% /var
tmpfs                  50460    121  50339    1% /run/user/1000
```

### 🌞 Déterminer la taille (avec du) et l'inode (avec ls) de chacun des fichiers suivants

```powershell 
aa@vbox:~$ ls -i /boot/vmlinuz* & du /boot/vmlinuz*
[1] 3248
260612 /boot/vmlinuz-5.10.0-32-amd64
6908 /boot/vmlinuz-5.10.0-32-amd64
aa@vbox:~$ which bash
/usr/bin/bash
aa@vbox:~$ ls -i /usr/bin/bash & du /usr/bin/bash
[1] 3276
392241 /usr/bin/bash
1208 /usr/bin/bash
aa@vbox:~$ ls -i /etc/passwd
171247 /etc/passwd
aa@vbox:~$ du /etc/passwd
4 /etc/passwd
```

### 🌞 Repérer le nom des nouveaux disques

```powershell

a@vbox:~$ lsblk
NAME          MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda             8:0    0   20G  0 disk
└─sda1          8:1    0   20G  0 part
  ├─zizi-root 254:0    0  9.3G  0 lvm  /
  ├─zizi-home 254:1    0  1.9G  0 lvm  [SWAP]
  ├─zizi-var  254:2    0  2.8G  0 lvm  /var
  └─zizi-swap 254:3    0  1.9G  0 lvm  /home
sdb             8:16   0   10G  0 disk
sdc             8:32   0   10G  0 disk
sr0            11:0    1 1024M  0 rom

```
### 🌞 Ajouter ces deux disques comme des PV LVM
```powershell

aa@vbox:~$ sudo pvcreate /dev/sdb
  Physical volume "/dev/sdb" successfully created.
aa@vbox:~$ sudo pvcreate /dev/sdc
  Physical volume "/dev/sdc" successfully created.


```

### 🌞 Créer un VG nommé cat

```powershell

aa@vbox:~$ sudo vgcreate cat /dev/sdb
  Volume group "cat" successfully created
aa@vbox:~$ sudo vgextend cat /dev/sdc
  Volume group "cat" successfully extended

  ```

### 🌞 Créer un LV nommé meoooow

```powershell
aa@vbox:~$ sudo lvcreate -L 3G cat -n meoooow
  Logical volume "meoooow" created.

```

### 🌞 Formater la partition (le LV) en autre chose que ext4

```powershell
aa@vbox:~$ sudo mkfs -t btrfs -f /dev/cat/meoooow
btrfs-progs v5.10.1
See http://btrfs.wiki.kernel.org for more information.

Label:              (null)
UUID:               7cfbc188-c503-4072-bc14-0170532e2536
Node size:          16384
Sector size:        4096
Filesystem size:    3.00GiB
Block group profiles:
  Data:             single            8.00MiB
  Metadata:         DUP             256.00MiB
  System:           DUP               8.00MiB
SSD detected:       no
Incompat features:  extref, skinny-metadata
Runtime features:  
Checksum:           crc32c
Number of devices:  1
Devices:
   ID        SIZE  PATH
    1     3.00GiB  /dev/cat/meoooow
```

### 🌞 Créer le point de montage /mnt/meow

```powershell
aa@vbox:~$ sudo mkdir /mnt/meow
```

### 🌞 Monter la partition meoooow sur le point de montage que vous avez créé

```powershell

aa@vbox:~$ sudo mount /dev/cat/meoooow /mnt/meow

```

### 🌞 Vérifier avec la commande adaptée que la partition est utilisable et qu'elle fait 3G

```powershell

aa@vbox:~$ sudo mount | grep /mnt/meow
/dev/mapper/cat-meoooow on /mnt/meow type btrfs (rw,relatime,space_cache,subvolid=5,subvol=/)
aa@vbox:~$ sudo lsblk | grep cat
└─cat-meoooow 254:4    0    3G  0 lvm  /mnt/meow
aa@vbox:~$ sudo mkdir /mnt/meow/jcp
aa@vbox:~$ sudo ls /mnt/meow
jcp

```

### 🌞 Il reste 2G sur le disque dur principal

```powershell
aa@vbox:~$ sudo lvextend -L+2G /dev/mapper/zizi-home
  Size of logical volume zizi/home changed from <1.86 GiB (476 extents) to <3.86 GiB (988 extents).
  Logical volume zizi/home successfully resized.
```