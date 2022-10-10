# ArchLinux
Arch Linux installieren, verschlüsseln und anpassen. [CODE](/CODE.md)
## 1. Basisinstallation
Laden Sie mit `loadkeys de` das deutsche Tastaturlayout. Starten Sie mit `iwctl` die Netzwerkeingabe.
```bash
station wlan0 connect [Netzwerk]
```
Geben Sie ihr Netzwerk und ihr Passwort ein. Mit `exit` beenden Sie das Programm. Mit `lsblk` lassen Sie sich alle Festplatten und ihr Partitionen anzeigen.
```bash
cfdisk /dev/[Festplatte]
```
Starten Sie die Partitionnierung ihrer Festplatte. Sie brauche eine `Root`-Partition und eine `SWAP`-Partition. Diese sollte ca. 4-8 GB groß sein. Falls Sie Arch auf einem `UEFI`-System installieren, brauchen Sie zusätzlich eine `EFI`-Partition, welche 300 MB groß ein sollte. Formatieren Sie anschließend die `Root`-Partition mit `ext4` und, falls vorhanden die `UEFI`-Partition mit `vfat`.
```bash
mkfs.ext4 /dev/[Root-Partition]
mkfs.vfat /dev/[EFI-Partition] # UEFI
```
Mounten Sie anschließend die `Root-Partiton`. Bei einer UEFI-Installation erstellen Sie anschließend das Verzeichniss `/boot` und mounten die `EFI-Partition` in dort hin.
```bash
mount /dev/[Root-Partition] /mnt
mount /dev/[EFI-Partition] /mnt/boot # UEFI
```
Anschließend installieren Sie das Grundbetriebssystem mit `nano` als Texteditor. Zusätzlich können Sie noch je nach CPU `intel-ucode` oder `amd-ucode`.
```bash
pacstrap /mnt base base-devel linux linux-firmware networkmanager nano
genfstab -U /mnt > /mnt/etc/fstab
```
Öffnen Sie als nächstes das installierte System mit `arch-chroot /mnt`. Öffnen Sie die Datei `/etc/nanorc` und aktivieren Sie `Syntax-Highlighting`.
Als nächstes legen Sie die Zeitzone fest
```bash
ln -s /usr/share/zoneinfo/Europe/Berlin /etc/localtime
nano /etc/locale.gen
```
Wählen Sie ihr Sprache aus und installieren diese mit `locale-gen`. 
```bash
echo „LANG=de_DE.UTF-8“ > /etc/locale.conf
echo „KEYMAP=de-latin1-nodeadkeys“ > /etc/vconsole.conf
echo „[Name]“ > /etc/hostname
mkinitcpio -P linux
```
Speichern Sie diese, wählen Sie ein Tastaturlayout und speichern den Hostname. Legen Sie mit `passwd` das Passwort für den `Root`-Nutzer fest. 
</br>

### 1.2 GRUB
```bash
pacman -S grub dosfstools gptfdisk git efibootmgr
```
Auf einem BIOS-System `GRUB` mit `grub-install /dev/[Festplatte]`, auf `UEFI`-Sytemen mit `grub-install --efi-directory=/boot /dev/[EFI-Festplatte]`. Öffnen Sie `/etc/default/grub` als `sudo` und aktivieren `OS-Prober` auf Dualboot-Systemen.
```bash
grub-mkconfig -o /boot/grub/grub.cfg
``` 
</br>

### 1.3 rEFInd
```bash
pacman -S refind dosfstools gptfdisk git efibootmgr
```
rEFInd mit `refind-install` installieren. In `/boot/refind_linux.conf` den Microcode integrieren. 
```bash
"Boot using default options"     "root=PARTUUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX rw add_efi_memmap initrd=boot\cpu_manufacturer-ucode.img initrd=boot\initramfs-%v.img"
``` 
</br>

`arch-chroot` mit `exit` verlassen. Danach mit `umount -R /mnt` die Dateisysteme auswerfen und anschließen mit `rebott 0` neustarten. Als nächstes den USB-Stick entfernen, das neue System starten und als `root` anmelden. </br>

### 1.4 Post-Installation Einstellungen
```bash
systemctl start NetworkManager
systemctl enable NetworkManager
nmcli device wifi connect [SSID] password [Passwort]
```
Netzwerk-Dienst starten und mit lokalem Netzwerk starten. Mit `pacman -Syy` die Paketquellen aktualisieren. Mit `useradd -m leon` und mit `usermod -aG wheel leon` zur Gruppe `wheel`.
```bash
EDITOR=/usr/bin/nano visudo
```
> `# %wheel ALL=(ALL) ALL` → aktivieren 

Mit `passwd leon` ein Passwort für `leon`festlegen und mit `chown -R leon:wheel /home/leon` Zugriff auf den `home`-Ordner freischalten. Abschließend den `root` abmelden und mir `leon`anmelden. </br>

## 2. verschlüsselte Installation von Arch Linux

## 3. AUR–Helper installieren

## 4. Desktopumgebung installieren

### 4.1 i3 Fenstermanager

### 4.2 Deepin Desktop Environment 

### 4.3 Xmonad
```bash
sudo pacman -Syy xorg lightdm lightdm-gtk-greeter xmonad xmonad-contrib xmobar dmenu picom nitrogen chromium alacritty nautilus
sudo systemctl enable lightdm
sudo nano /etc/lightdm/lightdm.conf
```

> `display-setup-script=xrandr --output Virtual-1 --mode [Auflösung]`
```bash
nano .xprofile
```
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
```haskell
import XMonad

main    =  xmonad def
           { terminal       =  „alacritty“
           , modMask     =  mod4Mask
           }
```

### 4.3.1 [DTOS](https://gitlab.com/dtos/dtos/)
<p align="center">
  <img src="https://thumbs2.imgbox.com/fc/eb/gi5uMar7_t.png" width="900px">
</p>
```bash
git clone https://gitlab.com/dtos/dtos
cd dtos/
./dtos
```