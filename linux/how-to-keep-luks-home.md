# How to reinstall system while keeping `/home` LUKS partition

reinstall using /boot and / and leave the /home partition alone during the install.
Once you have installed you can install cryptsetup,
set up your partition in /etc/crypttab and /etc/fstab and you'll be away.
I'll assume you have an encrypted swap For the details,
once you have installed and rebooted, open a terminal and:

```
# ubuntu
sudo apt-get install cryptsetup
# arch based
pacman --sync core/cryptsetup
```

```
cryptsetup luksOpen /dev/sdXX crypthome
cd /
mount -t ext4 /dev/mapper/crypthome /home
```

Edit the partition details and file system type as required.
Now you can browse /home and ensure it is what you expect.

> Then you need to put the following in /etc/crypttab
```
crypthome /dev/sda6 none         luks
cryptswap /dev/sda7 /dev/urandom swap
```

> And in /etc/fstab you need to add these lines to the end
```
/dev/mapper/crypthome  /home  ext4  defaults  0  2
/dev/mapper/cryptswap  none   swap  sw        0  0
```

Do a reboot to check it all works as expected and you're away :)
