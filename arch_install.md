Partitioning
cfdisk /dev/sda
I made a new partition and saw in archi wiki they recommended 550MB
then I changed the type to efi system.

and for the second partition i set up the root for the linux system.
Then i set it to the rest which is 19.5G.
then I write and quit to save it.

format partitions
mkfs
mkfs.fat -F32 /dev/sda1

then for root
mkfs.ext4 /dev/sda2

1.10
mount /dev/sda2 /mount
mkdir /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi

2.1-2.2

# pacstrap -K /mnt base linux linux-firmware

# pacstrap -K /mnt base base-devel to download base packages

3.1
i ran this command
`cfdisk`
then

```shell
genfstab -U /mnt >>
genfstab -U /mnt >> /mnt/etc/fstab
nano /mnt/etc/fstab
```

make sure to check resulting fstab file and edit if any errors

3.2
to change root into new system
I ran
`arch-chroot /mnt`

3.3 set time zone
find the time zones by
`cd /usr/share/zoneinfo`
and select correct zone.
then in croot
`ln -sf /usr/share/zoneinfo CSTCDT /etc/localtime`
and
`hwclock --systohc`

3.4

I had some issues using the nano command in chroot so I went online and saw that I must have
ommited it when doing the pacstrap stp.
Link: <https://bbs.archlinux.org/viewtopic.php?id=156519>

`pacman -S networkmanager`
to install networkmanager

`pacman -S man-db`
for man

I installed it in chroot using
`pacman -S nano`
then editted using
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
`echo Archlinux > /etc/hostname`

`ip address show`

`nano /etc/hosts`

```shell
127.0.0.1 localhost
::1 localhost
127.0.0.1 Archlinux.localdomain Archlinux
```

`systemctl enable NetworkManager`

3.6
i changed password with `passwd`

after finishing the documentation for archlinux i found a helpful youtube video for adding the plasma desktop environment.

i came upon an issue when installing zsh with pacman, it said packages weren't installed correctely due to packagekit timing out. I saw someone asking if they actually need packagekit, and I guess I don't? So i simply uninstalled it and problem was solved.

# assignment

cat /etc/passwd for users
ip addresss show

sudo cat /etc/sudoers
