# Wiki
    Installationsanleitung:
    https://wiki.archlinux.org/index.php/Installation_guide
    FAQ:
    https://wiki.archlinux.org/index.php/Frequently_asked_questions
    
#  Downloade eine aktuelles ISO von einem der offiziellen Servern auf:
https://www.archlinux.org/download/

#  Z.B.:
http://linux.rz.rub.de/archlinux/iso/2020.11.01/archlinux-2020.11.01-x86_64.iso

#  Überprüfe die Checksumme auf Windows in Powershell:
    CertUtil -hashfile C:\Users\chyr0_2\Downloads\archlinux-2020.11.01-x86_64.iso sha1
#  oder
    CertUtil -hashfile C:\Users\chyr0_2\Downloads\archlinux-2020.11.01-x86_64.iso md5
----
    Linux:
    md5sum <Pfad\zur\ISO>
---- 
    Hinweis: das bootstrap-iso ist für eine Installation aus einer existierenden Linux Installation
    Das "normale" iso (oben verlinkt) zum Erstellen eines Live-Mediums z.B. mit dd oder Unetbootin oder multibootusb. 
    https://unix.stackexchange.com/questions/224738/which-arch-linux-file-do-i-download
---
