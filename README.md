# Instalación y configuración de LVM en servidor creado por Vagrant
#### Las herramientas que se usará serán las siguientes:
- [Vagrant](https://www.vagrantup.com/downloads)
- [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
---
### Crearemos la máquina virtual con Vagrant.
1. Crear carpeta:
~~~
$ mkdir vagrant_lvm
~~~
2. Dentro de la carpeta:
~~~
$ cd vagrant_lvm
~~~
3. Clonar este repositorio:
~~~
$ git clone https://github.com/mariocampos/Vagrant-LVM.git
~~~
4. Arrancar y levantar la máquina con el siguiente comando:
~~~
$ vagrant up
Bringing machine 'vm1' up with 'virtualbox' provider...
==> vm1: Checking if box 'centos/7' version '2004.01' is up to date...
==> vm1: Clearing any previously set forwarded ports...
==> vm1: Fixed port collision for 22 => 2222. Now on port 2201.
==> vm1: Clearing any previously set network interfaces...
==> vm1: Preparing network interfaces based on configuration...
    vm1: Adapter 1: nat
    vm1: Adapter 2: hostonly
==> vm1: Forwarding ports...
    vm1: 22 (guest) => 2201 (host) (adapter 1)
==> vm1: Running 'pre-boot' VM customizations...
==> vm1: Booting VM...
==> vm1: Waiting for machine to boot. This may take a few minutes...
    vm1: SSH address: 127.0.0.1:2201
    vm1: SSH username: vagrant
    vm1: SSH auth method: private key
    vm1: Warning: Connection aborted. Retrying...
    vm1: Warning: Connection reset. Retrying...
    vm1: Warning: Connection aborted. Retrying...
    vm1: Warning: Connection reset. Retrying...
    vm1:
    vm1: Vagrant insecure key detected. Vagrant will automatically replace
    vm1: this with a newly generated keypair for better security.
    vm1:
    vm1: Inserting generated public key within guest...
    vm1: Removing insecure key from the guest if it's present...
    vm1: Key inserted! Disconnecting and reconnecting using new SSH key...
==> vm1: Machine booted and ready!
[vm1] No Virtualbox Guest Additions installation found.
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.orbyta.com
 * extras: mirror.orbyta.com
 * updates: mirror.orbyta.com
Resolving Dependencies
--> Running transaction check
---> Package centos-release.x86_64 0:7-8.2003.0.el7.centos will be updated
---> Package centos-release.x86_64 0:7-9.2009.1.el7.centos will be an update
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package             Arch        Version                     Repository    Size
================================================================================
Updating:
 centos-release      x86_64      7-9.2009.1.el7.centos       updates       27 k

Transaction Summary
================================================================================
Upgrade  1 Package

Total download size: 27 k
Downloading packages:
No Presto metadata available for updates
Public key for centos-release-7-9.2009.1.el7.centos.x86_64.rpm is not installed
warning: /var/cache/yum/x86_64/7/updates/packages/centos-release-7-9.2009.1.el7.centos.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
Retrieving key from file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Importing GPG key 0xF4A80EB5:
 Userid     : "CentOS-7 Key (CentOS 7 Official Signing Key) <security@centos.org>"
 Fingerprint: 6341 ab27 53d7 8a78 a7c2 7bb1 24c6 a8a7 f4a8 0eb5
 Package    : centos-release-7-8.2003.0.el7.centos.x86_64 (@anaconda)
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Updating   : centos-release-7-9.2009.1.el7.centos.x86_64                  1/2
  Cleanup    : centos-release-7-8.2003.0.el7.centos.x86_64                  2/2
  Verifying  : centos-release-7-9.2009.1.el7.centos.x86_64                  1/2
  Verifying  : centos-release-7-8.2003.0.el7.centos.x86_64                  2/2

Updated:
  centos-release.x86_64 0:7-9.2009.1.el7.centos

Complete!
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.orbyta.com
 * extras: mirror.orbyta.com
 * updates: mirror.orbyta.com
No package kernel-devel-3.10.0-1127.el7.x86_64 available.
Error: Nothing to do
Unmounting Virtualbox Guest Additions ISO from: /mnt
umount: /mnt: not mounted
==> vm1: Checking for guest additions in VM...
    vm1: No guest additions were detected on the base box for this VM! Guest
    vm1: additions are required for forwarded ports, shared folders, host only
    vm1: networking, and more. If SSH fails on this machine, please install
    vm1: the guest additions and repackage the box to continue.
    vm1:
    vm1: This is not an error message; everything may continue to work properly,
    vm1: in which case you may ignore this message.
The following SSH command responded with a non-zero exit status.
Vagrant assumes that this means the command failed!

umount /mnt

Stdout from the command:



Stderr from the command:

umount: /mnt: not mounted

~~~
5. Nos conectamos a la VM y validamos que tenga más de un disco creado:
~~~
$ vagrant ssh
[vagrant@localhost ~]$ lsblk
NAME   MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda      8:0    0  40G  0 disk
└─sda1   8:1    0  40G  0 part /
sdb      8:16   0   5G  0 disk
sdc      8:32   0   5G  0 disk
sdd      8:48   0   5G  0 disk
sde      8:64   0   5G  0 disk
sdf      8:80   0   5G  0 disk

~~~
6. Luego cambiamos al usuario *root* y actualizamos los paquetes:
~~~
[vagrant@localhost ~]$ sudo su -
[root@localhost ~]# yum update -y
.
.
. 
Complete!
[root@localhost ~]#
~~~
7. Ahora instalamos *LVM*.
~~~
[root@localhost ~]# yum install lvm2
.
.
.
Complete!
~~~
### Instalando LVM
8. Creamos la particion con el siguiente comando:
~~~
[root@localhost ~]# fdisk /dev/sdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0xa54d769d.

Command (m for help):
~~~
9. Ponemos "o", para crear una paticion vacía.
~~~
Command (m for help): o
Building a new DOS disklabel with disk identifier 0x22cb40cc.

Command (m for help):
~~~
10. Ahora ponemos "n", para que cree una partición nueva y presionamos enter para crear la partición ente caso por defecto.
~~~
Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p):
Using default response p
Partition number (1-4, default 1):
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
Partition 1 of type Linux and of size 5 GiB is set
~~~
11. Luego presionamos "t" para cambiar el tipo de la partición que en este caso será hexadecimal "8e".
~~~
Command (m for help): t
Selected partition 1
Hex code (type L to list all codes): 8e
Changed type of partition 'Linux' to 'Linux LVM'
~~~
12. Digitamos "w" para escribir los cambios (guardar).
~~~
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
[root@localhost ~]#
~~~
13. Creamos un grupo de volumen, en este caso le pondré el grupo "unit".
~~~
[root@localhost ~]# vgcreate unit /dev/sdb1
  Volume group "unit" successfully created
~~~
>Se puede crear un VG(Grupo de Volumen) como tantos PV(Volumen Físico) posibles.
14. Ejecutamos lo siguiente para validar lo creado.
~~~
[root@localhost ~]# vgdisplay unit
  --- Volume group ---
  VG Name               unit
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <5.00 GiB
  PE Size               4.00 MiB
  Total PE              1279
  Alloc PE / Size       0 / 0
  Free  PE / Size       1279 / <5.00 GiB
  VG UUID               7UhLz3-3dC0-Tg48-aE03-9YYT-8j6G-8ITqzq
~~~
15. Creamos un volumen lógico, le asignamos 1G de espacio y le ponemos el nombre "compartido_lvm".
~~~
[root@localhost ~]# lvcreate --size 1G --name compartido_lvm unit
  Logical volume "compartido_lvm" created.
~~~
16. Validamos lo creado:
~~~
[root@localhost ~]# lvdisplay /dev/unit/compartido_lvm
  --- Logical volume ---
  LV Path                /dev/unit/compartido_lvm
  LV Name                compartido_lvm
  VG Name                unit
  LV UUID                QWfxRE-RBnQ-CSV5-LSwC-72wq-F6eq-kbe9Y5
  LV Write Access        read/write
  LV Creation host, time localhost.localdomain, 2021-07-26 20:41:08 +0000
  LV Status              available
  # open                 0
  LV Size                1.00 GiB
  Current LE             256
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0
~~~
17. Formateamos el volumen lógico creado y le asignamos un sistema de archivos "ext4".
~~~
[root@localhost ~]# mkfs.ext4 /dev/unit/compartido_lvm
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
65536 inodes, 262144 blocks
13107 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=268435456
8 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376

Allocating group tables: done
Writing inode tables: done
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done
~~~
18. Creamos un punto de montaje:
~~~
[root@localhost ~]# mkdir -pv /var/prueba/prueba_lvm
mkdir: created directory ‘/var/prueba’
mkdir: created directory ‘/var/prueba/prueba_lvm’
~~~
19. Ahora montamos el volumen creado con el punto de montaje.
~~~
[root@localhost ~]# mount /dev/unit/compartido_lvm /var/prueba/prueba_lvm
~~~
20. Luego ejecutamos el siguiente comando para aumentar espacio al montaje creado.
- Primero revisamos cuanto tiene de espacio:
~~~
[root@localhost ~]# df -h
Filesystem                       Size  Used Avail Use% Mounted on
devtmpfs                         111M     0  111M   0% /dev
tmpfs                            118M     0  118M   0% /dev/shm
tmpfs                            118M  8.5M  109M   8% /run
tmpfs                            118M     0  118M   0% /sys/fs/cgroup
/dev/sda1                         40G  3.5G   37G   9% /
tmpfs                             24M     0   24M   0% /run/user/1000
/dev/mapper/unit-compartido_lvm  976M  2.6M  907M   1% /var/prueba/prueba_lvm
~~~
- Luego ejecutamos el comando para aumentarle 1G más de espacio.
~~~
[root@localhost ~]# lvextend --size +1G --resizefs unit/compartido_lvm
  Size of logical volume unit/compartido_lvm changed from 1.00 GiB (256 extents) to 2.00 GiB (512 extents).
  Logical volume unit/compartido_lvm successfully resized.
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/mapper/unit-compartido_lvm is mounted on /var/prueba/prueba_lvm; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on /dev/mapper/unit-compartido_lvm is now 524288 blocks long.
~~~
- Validamos que en la última línea aumentó de 976M a 2.0G.
~~~
[root@localhost ~]# df -h
Filesystem                       Size  Used Avail Use% Mounted on
devtmpfs                         111M     0  111M   0% /dev
tmpfs                            118M     0  118M   0% /dev/shm
tmpfs                            118M  8.5M  109M   8% /run
tmpfs                            118M     0  118M   0% /sys/fs/cgroup
/dev/sda1                         40G  3.5G   37G   9% /
tmpfs                             24M     0   24M   0% /run/user/1000
/dev/mapper/unit-compartido_lvm  2.0G  3.0M  1.9G   1% /var/prueba/prueba_lvm
~~~
