# Arch Linux installieren
```bash
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
nano /etc/nanorc    # Syntax-Highlighting aktivieren
ln -s /usr/share/zoneinfo/Europe/Berlin /etc/localtime
nano /etc/locale.gen    # Sprache auswählen
locale-gen
echo „LANG=de_DE.UTF-8“ > /etc/locale.conf
echo „KEYMAP=de-latin1-nodeadkeys“ > /etc/vconsole.conf
echo „[Name]“ > /etc/hostname
passwd
mkinitcpio -P linux
pacman -S grub dosfstools gptfdisk git efibootmgr
```
| BIOS                           | UEFI                                                     |
| ------------------------------ | -------------------------------------------------------- |
| `grub-install /dev/[Festplatte]` | `grub-install --efi-directory=/boot /dev/[EFI-Festplatte]` |
```bash
sudo nano /etc/default/grub   # OS-Prober aktivieren
grub-mkconfig -o /boot/grub/grub.cfg
exit
umount -R /mnt
reboot 0
```
> USB-Stick entfernen, neues System starten, als `root` anmelden
```bash
systemctl start NetworkManager
systemctl enable NetworkManager
nmcli device wifi connect [SSID] password [Passwort]
pacman -Syy
useradd -m leon
usermod -aG wheel leon
EDITOR=/usr/bin/nano visudo
   # %wheel ALL=(ALL) ALL → aktivieren
passwd leon
chown -R leon:wheel /home/leon
```
> `root` abmelden, mit `leon` anmelden
<br />

## Arch Linux – verschlüsselte Installation
```bash
cfdisk /dev/[Festplatte]
```
> - EFI-Partition – 300MB
> - Kernel-Partition – 300MB
> - LUKS-Partition
```bash
mkfs.msdos -F 32 /dev/[EFI-Partition]
mkfs.ext4 /dev/[Kernel-Partition]
cryptsetup -v -y --cipher aes-xts-plain64 --key-size 256 --hash sha256 --iter-time 2000 --use-urandom --verify-passphrase luksFormat /dev/[LUKS-Partition] 
cryptsetup open /dev/[LUKS-Partition] LEON
mkfs.ext4 /dev/mapper/LEON
mount /dev/mapper/LEON /mnt
mkdir /mnt/boot
mount /dev/[Kernel-Partition] /mnt/boot
mkdir /mnt/boot/efi
mount /dev/[EFI-Partition] /mnt/boot/efi
pacman -S wpa_supplicant dialog dhcpcd
nano /etc/mkinitcpio.conf
HOOKS={encrypt} hinzufügen, vor filesystems
nano /etc/default/grub
```
> GRUB_CMDLINE_LINUX=“cryptdevice=/dev/[LUKS-Partition]:LEON root=/dev/mapper/LEON“ </br>
> → GRUB.cfg-Datei erstellen
```bash
systemctl enable dhcpcd
umount -R /mnt
cryptsetup close LEON
```
</br>

## AUR – Helper installieren
```bash
cd /opt
sudo git clone https://aur.archlinux.org/yay.git
sudo chown -R leon:wheel ./yay
makepkg -si
sudo yay -S pamac
```
</br>

## Desktopumgebung installieren
```bash
sudo pacman -S [xfce4, tilix | kde-plasma-desktop] lightdm lightdm-gtk-greeter xorg xorg-server xorg-xinit
sudo systemctl enable lightdm
sudo nano /etc/lightdm/lightdm.conf
```
> greeter-session=lightdm-gtk-greeter → aktivieren </br>
> → neustarten
</br>

### [bspwm](https://github.com/Pfeffimann18/bspwm)
</br>

### [i3 Fenstermanager](https://github.com/Pfeffimann18/i3)
</br>

### Deepin Desktop Environment 
```bash
sudo pacman -S xf86-video-amdgpu xorg deepin deepin-extra chromium 
sudo nano /ext/lightdm/lightdm.conf
```
> greeter-session=lightdm-deepin-greeter
> display-setup-script=xrand --mode [Auflösung]
```bash
sudo systemctl enable lightdm
reboot
```
</br>

### Xmonad – Setup
```bash
sudo pacman -Syy xorg lightdm lightdm-gtk-greeter xmonad xmonad-contrib xmobar dmenu picom nitrogen chromium xfce4-terminal nautilus
sudo systemctl enable lightdm
sudo nano /etc/lightdm/lightdm.conf
display-setup-script=xrandr –output Virtual-1 –mode [Auflösung]
nano .xprofile
```
> .xprofile
```bash
# Keyboard Layout
setxbmap de &
# Wallpaper
nitrogen --restore &
#Compositor
picom -f &
```
```bash
mkdir .xmonad
cd .xmonad
nano xmonad.hs
```
> xmonad.hs
```haskell
import XMonad

main    =  xmonad def
           { terminal       =  „xfce4-terminal“
           , modMask     =  mod4Mask
           }
```
