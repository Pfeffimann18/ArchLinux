# ArchLinux
Arch Linux installieren, verschlüsseln und anpassen.
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
```
Speichern sie diese, wählen sie ein Tastaturlayout und speichern den Hostname. Legen sie mit `passwd` das Passwort für den `Root`-Nutzer fest.
