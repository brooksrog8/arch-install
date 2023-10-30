# 1.9

# Partitioning

When recognized by the live systeem, identify the block devices by using `lsblk`
then
`cfdisk /dev/sda`
to see current devices.
Make a new partition by selecting new and set it to the recommended 550mb and make sure its type is set to EFI system.
For the second partition, use the rest of the space you have and make sure the type is set to linux filesystem.

# format partitions

format the first efi partition by using `mkfs.fat -F32 /dev/sda1`

then for filesystem
`mkfs.ext4 /dev/sda2`

# 1.10.

# Mounting file systems

simply mount the devices by using the following commands

```shell
mount /dev/sda2 /mount
mkdir /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
```

# 2.1-2.2 Selecting mirrors and install essential packages

nano into /etc/pacman.d

```shell
 pacstrap -K /mnt base linux linux-firmware

 pacstrap -K /mnt base base-devel to download base packages
```

# 3.1 Fstab

i ran this command
`cfdisk`
to look at devices
then

```shell
genfstab -U /mnt >>
genfstab -U /mnt >> /mnt/etc/fstab
nano /mnt/etc/fstab
```

make sure to check resulting fstab file in nano and edit if any errors

3.2
to change root into new system
I ran

`arch-chroot /mnt`

3.3 set time zone
find the time zones by using the command:

`cd /usr/share/zoneinfo`
and select correct zone.
then in croot
`ln -sf /usr/share/zoneinfo CSTCDT /etc/localtime`

and

`hwclock --systohc`

3.4

I had some issues using the nano command in chroot so I went online and saw that I must have
ommited it when doing the pacstrap step.
Link: <https://bbs.archlinux.org/viewtopic.php?id=156519>
I then also installed the other commands I will be needing.

`pacman -S networkmanager`
to install networkmanager

`pacman -S man-db`
for man

editted /etc/locale.gen using
`nano /etc/local.gen `

and uncommented en_US.UTF-8 UTF-8
then ran
`locale-gen`

`nano /etc/local.conf`
entered
`LANG=en_US.UTF-8`
into the file and saved

3.5

create hostname file
using the commands
`echo Archlinux > /etc/hostname`

`ip address show`

`nano /etc/hosts`

```shell
127.0.0.1 localhost
::1 localhost
127.0.0.1 Archlinux.localdomain Archlinux
```

# MAKE SURE NETWORK MANAGER IS INSTALLED

install networkmanager and enable it using the command

```shell
`pacman -S networkmanager`
`systemctl enable NetworkManager`
```

3.6
i changed password with `passwd`

after finishing the documentation for archlinux i found a helpful youtube video for adding the plasma desktop environment. At the 18:07 minute mark
Link=<https://www.youtube.com/watch?v=FFXRFTrZ2Lk&ab_channel=EF-LinuxMadeSimple>

i came upon an issue when installing zsh with pacman, it said packages weren't installed correctely due to packagekit timing out. I saw someone asking if they actually need packagekit... and thought to myself, I guess I don't? So i simply uninstalled it and problem was solved.

# assignment

cat /etc/passwd for users
ip addresss show

sudo cat /etc/sudoers
