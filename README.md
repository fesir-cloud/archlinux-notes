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
    
                    Größe           Typ
    Partition 1:    1M //512MiB     1  (EFI System)
    Partition 2:    9M //4GB        19 (Linux Swap)
    Partition 3:    Rest            24 (Linux root (x86_64))
    
In Zukunft werde ich mich mit Btrfs auseinandersetzen müssen um mehrere Rollbacks zu erzeugen, falls mal irgendein Update schiefgeht.

----    
    
    
    
