# Erstellen des Boot-Mediums:

    Installationsanleitung:
    https://wiki.archlinux.org/index.php/Installation_guide
    FAQ:
    https://wiki.archlinux.org/index.php/Frequently_asked_questions
    
Downloade eine aktuelles ISO von einem der offiziellen Servern auf:
https://www.archlinux.org/download/

    Guter Mirror: http://linux.rz.rub.de/archlinux/iso/2020.11.01/archlinux-2020.11.01-x86_64.iso

----

Überprüfe die Checksumme auf Windows in Powershell:

    CertUtil -hashfile C:\Users\chyr0_2\Downloads\archlinux-2020.11.01-x86_64.iso sha1

oder

    CertUtil -hashfile C:\Users\chyr0_2\Downloads\archlinux-2020.11.01-x86_64.iso md5

oder

    Linux:    md5sum <Pfad\zur\ISO>

---- 

    Hinweis: das bootstrap-iso ist für eine Installation aus einer existierenden Linux Installation
    Das "normale" iso (oben verlinkt) zum Erstellen eines Live-Mediums z.B. mit:
    # dd aus coreutils (Linux)
    oder 
    # dd aus multibootusb (Windows) unter "Write Image to Disk"
    Quelle: https://unix.stackexchange.com/questions/224738/which-arch-linux-file-do-i-download

----

# Dann eventuellen secureboot deaktivieren und über BIOS oder UEFI das Medium booten.

----

Richtige Tastatur einstellen:

    loadkeys de-latin1
    
    Leider finde ich kein german-qwerty !!!!

UEFI Boot Modus überprüfen:

    ls /sys/firmware/efi/efivars && echo $?
    
Wenn die Fehlervariable($?) 0 ist, ist derr EFI-Boot-Test bestanden.
    
----

# Setup der Internetverbindung:

Liste alle Netzwerkgeräte auf:

    ip link

Überprüfe ob rfkill eines der Netzwerkgeräte soft-/oder hardblokiert:
    
    https://wiki.archlinux.org/index.php/Network_configuration/Wireless#Rfkill_caveat

    rfkill list
    
Wenn ja z.B.:
    
    rfkill unblock wifi
    
 Wlan-Setup:
 
https://wiki.archlinux.org/index.php/Iwd#iwctl 
(der mittels ip link herausgefundenen Name des Wlan-Gerätes ist wlan0)
     
    iwctl
    
    device list    //Überprüfen ob der Wlan-Adapter auch im Station-Mode läuft.
    
    station wlan0 scan    //Scannt alle sichtbaren Netzwerke.
    
    station wlan0 get-networks    //Zeigt alle gerade gescannten Netzwerke an.
    
    station wlan0 connect <SSID>    //Hierauf wird für das Passwort gepromptet.
    
    exit
    
    ping 8.8.8.8    //Pingt den Google-DNS-server 8.8.8.8 an um die Verbindung zu testen.
    
# Zeitsynchronisationsüberprüfung:

    timedatectl set-ntp true    //Stellt die Verbindung zum vorgewählten Zeitserver her und synchronisiert
    
    timedatectl list-timezones    //Listet alle Zeitzonen auf -> Europe/Berlin
    
    timedatectl set-timezone Europe/Berlin    //Macht das offensichtliche.

# Partitionierung der Festplatte:

    fdisk -l    //Zeige alle vorhandenen Blockdevices(Datenspeichergeräte) -> /dev/sda
    
    fdisk /dev/sda    //Starte fdisk zum Partitionieren des gewählten Blockdevices
    
    
----
Beispielpartitionierung:
    
                    Größe           Typ                         Format
    Partition 1:    1M //512MiB     1  (EFI System)             
    Partition 2:    9M //4GB        19 (Linux Swap)             swapon
    Partition 3:    Rest            24 (Linux root (x86_64))    ext4
    
    
Alternativ:

                    Größe           Typ
    Partition 1:    550MiB (+550M)  1  (EFI System)             FAT32
    Partition 2:    9M (+2G)        19 (Linux Swap)             swapon
    Partition 3:    Rest            20 (Linux filesystem)       ext4
    
    
Es wird Partitioniert indem fdisk benutzt wir, wie im folgenden erklärt:

    fdisk /dev/sda
    Command: p      //Zeige aktuelle Partitionstabelle
    Command: n      //Neuse Partition gefolgt von der Definition der Partition über Erster und letzter Sektor, sowie Typ der Partition
    Command: w      //Schreiben der Tabelle
    Command: a      //Bootflag
    Command: m      //help
    
    
In Zukunft werde ich mich mit Btrfs auseinandersetzen müssen um mehrere Rollbacks zu erzeugen, falls mal irgendein Update schiefgeht.

----    
    
Formatierung der gerade erstellten Partitionen:


    mkfs.fat -F32 /dev/sda1 //EFI-Partition als FAT32 partitionieren.

    mkfs.ext4 /dev/sda3     //Root-Partitionals Ext4 formatieren.
    
    mkswap /dev/sda2        //SWAP-Partition als Linuxswap Partitionieren
    
Einbinden der gerade Formatierten Partitionen:

    mount /dev/sda3 /mnt    //Die Root-Partition unter /mnt einbinden.
    
    swapon /dev/sda2        //SWAP aktivieren
    
Mit lsblk überprüfen ob  alles funktioniert hat.
    
    
# Installation:

    https://wiki.archlinux.org/index.php/installation_guide#Install_essential_packages
    
    pacstrap /mnt base linux linux-firmware     //Die drei Pakete base, linux & linux-firmware in /mnt downloaden. Kann je nach Internetverbindung etwas dauern...(das St. Joseph Patienten-Wlan ist denkbar ungeeignet, weswegen ein Handy-Hotspot verwendet wurde...somit immerhon ~400-1000KiB/s -> 10:06 Minuten:Sekunden total)

--- 

fstab-Datei mit den Richtigen Daten füttern:

    genfstab -U /mnt >> /mnt/etc/fstab      //Einträge in die Filesystem-Tabelle automatisch mit UUID generieren.


Wechseln des Root-Verszeichnisses auf die als /mnt gemountete /dev/sda 

    arch-chroot /mnt                        //Root-Verzeichnis auf /mnt ändern: danach ist "/mnt" = "/"


Zeitsynchronisation:

    ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime

    hwclock --systohc  


Installation von nano und VIM sowie Einstellung der Locals:

     pacman -S nano vim
     
     echo de_DE.UTF-8 UTF-8 >> /etc/locale.gen      //Diese Zeile kann auch einfach mit nano oder VIM von dem führenden '#' Zeichen befreit und damit aktiviert werden. 

     locale-gen                                     //Gerade gewählte Localisierung aktivieren: Es ist erkennbar an der Ausgabe dieses Befehles ob es funktioniert hat.
     
     echo LANG=de_DE.UTF-8 > /etc/locale.conf       //Sprach-Codec persistieren.
     
     echo KEYMAP=de-latin1 > /etc/vconsole.conf     //Tastaturlayout persistieren.


Netzwerkkonfiguration und Initramfs erstellen:

    printf "127.0.0.1    localhost\n::1    localhost\n127.0.1.1     chyr0.localdomain	chyr0" > /etc/hosts    //identifiziert 127.0.0.1 und ::1 als localhost

    echo chyr0 > /etc/hostname                                          //Schreibt den Hostnamen in die entsprechende Datei.

    mkinitcpio -P                                                       //Erstellt ein Initalesl-RAM-Dateisystem.
    
RootPasswort festlegen:    
    
    passwd                                                              //Als letztes sollte noch ein Rootpasswort festgelegtt werden.
    
Anlegen eines Nutzerprofiles und hoinzufügen der Benutzers zu den wichtigen Gruppen:
    
    useradd -m chyr0
    
    passwd chyr0
    
    usermod -aG wheel,audio,vider,optical,storage chyrone
    
Installieren des sudo Befehls und hinzufügen des Privilegs des Ausführens aller Kommandos mit sudo für Mitglieder der wheel Gruppe:

    pacman -Sy sudo
    
    printf "%wheel ALL=(ALL) ALL" >> visudo
    
    
Bootloader:    
    
    pacman -S grub
    
    pacman -S efibootmgr dosfstools os-prober mtools
    
    mkdir /boot/EFI
    
    mount /dev/sda1 /boot/EFI
    
    grub-install --target=x86_64-efi --bootloader-id=grub_uefi --recheck

    grub-mkconfig mkconfig -o /boot/grub/grub.cfg


Nun ist es vollbracht. EIn funktionale Arch-Linux ist nun installiert. Zum Schluss noch einmal die CHROOT treten (Strg + D) oder exit zum Verlassen derselben und umount -R /mnt und um das gerade Installierte Dateisystem auszuwerfen, und dann sollte ein Reboot in das gerade erstellt System Booten.

Es ist aber ratsam jetzt schon ein par weitere Pakete zu installieren, da hier einfach die Möglichkeit dessen besteht:

    pacman -S networkmanager vim
    
SystemD-Einheit des NetworkManagers aktivieren:    
    
    systemctl enable NetworkManager    
    
    
Jetzt aber wirklich:

    exit
    
    umount -l /mnt
    
    






