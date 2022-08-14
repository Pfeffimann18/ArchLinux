# ArchLinux
Arch Linux installieren, verschlüsseln und anpassen. [CODE](/CODE.md)
## 1. Basisinstallation
Laden sie mit `loadkeys de` ein deutsches Tastaturlayout. Starten sie mit `iwctl` die Netzwerkeingabe.
```
station wlan0 connect [Netzwerk]
```
Geben sie ihr Netzwerk und ihr Passwort ein. Mit `exit` beenden sie das Programm. Mit `lsblk` lassen sie sich alle Festplatten und ihr Partitionen anzeigen.
```
cfdisk /dev/[Festplatte]
```
Starten sie die Partitionnierung ihrer Festplatte. Sie brauche eine `Root`-Partition und eine `SWAP`-Partition. Diese sollte ca. 4-8 GB groß sein. Falls sie Arch auf einem `UEFI`-System installieren, brauchen sie zusätzlich eine `EFI`-Partition, welche 300 MB groß ein sollte. Formatieren sie anschließend die `Root`-Partition mit `ext4` und, falls vorhanden die `UEFI`-Partition mit `vfat`.
```
mkfs.ext4 /dev/[Root-Partition]
mkfs.vfat /dev/[EFI-Partition] # UEFI
```
Mounten sie anschließend die `Root-Partiton`. Bei einer UEFI-Installation erstellen sie anschließend das Verzeichniss `/boot` und mounten die `EFI-Partition` in dort hin.
```
mount /dev/[Root-Partition] /mnt
mount /dev/[EFI-Partition] /mnt/boot # UEFI
```
Anschließend installieren sie das Grundbetriebssystem mit `nano` als Texteditor. Zusätzlich können sie noch je nach CPU `intel-ucode` oder `amd-ucode`.
```
pacstrap /mnt base base-devel linux linux-firmware networkmanager nano
genfstab -U /mnt > /mnt/etc/fstab
```
Öffnen sie als nächstes das installierte System mit `arch-chroot /mnt`. Öffnen sie die Datei `/etc/nanorc` und aktivieren sie `Syntax-Highlighting`.
Als nächstes legen sie die Zeitzone fest
```
ln -s /usr/share/zoneinfo/Europe/Berlin /etc/localtime
nano /etc/locale.gen
```
Wählen sie ihr Sprache aus und installieren diese mit `locale-gen`. 
```
echo „LANG=de_DE.UTF-8“ > /etc/locale.conf
echo „KEYMAP=de-latin1-nodeadkeys“ > /etc/vconsole.conf
echo „[Name]“ > /etc/hostname
mkinitcpio -P linux
```
Speichern sie diese, wählen sie ein Tastaturlayout und speichern den Hostname. Legen sie mit `passwd` das Passwort für den `Root`-Nutzer fest. 
</br>

### 1.3 GRUB
```
pacman -S grub dosfstools gptfdisk git efibootmgr
```
Auf einem BIOS-System `GRUB` mit `grub-install /dev/[Festplatte]`, auf `UEFI`-Sytemen mit `grub-install --efi-directory=/boot /dev/[EFI-Festplatte]`. Öffnen sie `/etc/default/grub` als `sudo` und aktivieren `OS-Prober` auf Dualboot-Systemen.
```
grub-mkconfig -o /boot/grub/grub.cfg
``` 
</br>

### 1.2 rEFInd
```
pacman -S refind dosfstools gptfdisk git efibootmgr
```
rEFInd mit `refind-install` installieren. In `/boot/refind_linux.conf` den Microcode integrieren. 
```
"Boot using default options"     "root=PARTUUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX rw add_efi_memmap initrd=boot\cpu_manufacturer-ucode.img initrd=boot\initramfs-%v.img"
``` 
</br>

`arch-chroot` mit `exit` verlassen. Danach mit `umount -R /mnt` die Dateisysteme auswerfen und anschließen mit `rebott 0` neustarten. Als nächstes den USB-Stick entfernen, das neue System starten und als `root` anmelden. </br>

### 1.3 Post-Installation Einstellungen
```
systemctl start NetworkManager
systemctl enable NetworkManager
nmcli device wifi connect [SSID] password [Passwort]
```
Netzwerk-Dienst starten und mit lokalem Netzwerk starten. Mit `pacman -Syy` die Paketquellen aktualisieren. Mit `useradd -m leon` und mit `usermod -aG wheel leon` zur Gruppe `wheel`.
```
EDITOR=/usr/bin/nano visudo
```
> `# %wheel ALL=(ALL) ALL` → aktivieren 

Mit `passwd leon` ein Passwort für `leon`festlegen und mit `chown -R leon:wheel /home/leon` Zugriff auf den `home`-Ordner freischalten. Abschließend den `root` abmelden und mir `leon`anmelden. </br>

## 2. verschlüsselte Installation von Arch Linux

## 3. AUR–Helper installieren

## 4. Desktopumgebung installieren

### 4.1 i3 Fenstermanager

### 4.2 [DTOS](https://gitlab.com/dtos/dtos/)
```
git clone https://gitlab.com/dtos/dtos
cd dtos/
./dtos
```

### 4.3 Deepin Desktop Environment 

### 4.4 Xmonad
```
sudo pacman -Syy xorg lightdm lightdm-gtk-greeter xmonad xmonad-contrib xmobar dmenu picom nitrogen chromium alacritty nautilus
sudo systemctl enable lightdm
sudo nano /etc/lightdm/lightdm.conf
```

> `display-setup-script=xrandr –output Virtual-1 –mode [Auflösung]`
```
nano .xprofile
```
```
# Keyboard Layout
setxbmap de &
# Wallpaper
nitrogen --restore &
#Compositor
picom -f &
```
```
mkdir .xmonad
cd .xmonad
nano xmonad.hs
```
```
import XMonad

main    =  xmonad def
           { terminal       =  „alacritty“
           , modMask     =  mod4Mask
           }
```
