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
Starten sie die Partitionnierung ihrer Festplatte. Sie brauche eine Root-Partition und eine SWAP-Partition. Diese sollte ca. 4-8 GB groß sein. Falls sie Arch auf einem UEFI-System installieren,brauchen sie zusätzlich eine EFI-Partition, welche 300 MB groß ein sollte. 
