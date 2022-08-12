# Arch Linux installieren
```
loadkeys de
iwctl
station wlan0 connect [Netzwerk]
lsblk
cfdisk /dev/[Festplatte]
mkfs.ext4 /dev/[Root-Partition]
UEFI: mkfs.vfat /dev/[EFI-Partition; 300 MB]
mount /dev/[Root-Partition] /mnt
pacstrap /mnt base base-devel linux linux-firmware networkmanager nano intel-ucode
genfstab -U /mnt > /mnt/etc/fstab
arch-chroot /mnt
nano /etc/nanorc → Syntax-Highlighting aktivieren
ln -s /usr/share/zoneinfo/Europe/Berlin /etc/localtime
nano /etc/locale.gen	→ Sprache auswählen
locale-gen
echo „LANG=de_DE.UTF-8“ > /etc/locale.conf
echo „KEYMAP=de-latin1-nodeadkeys“ > /etc/vconsole.conf
echo „[Name]“ > /etc/hostname
passwd
mkinitcpio -P linux
pacman -S grub dosfstools gptfdisk git efibootmgr
BIOS: grub-install /dev/[Festplatte] 
UEFI: grub-install --efi-directory=/boot /dev/[EFI-Festplatte]
sudo nano /etc/default/grub	→ OS-Prober aktivieren
grub-mkconfig -o /boot/grub/grub.cfg
exit
umount -R /mnt
reboot 0
```
