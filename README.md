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
$ git clone 
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

~~~
9. 
