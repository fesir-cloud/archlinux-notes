# Xorg und Window Manager Installation:

----
----
----

Bitte folgendes Setup als user in ~ ausführen, entweder direkt als user anmelden, oder: 

    sudo chyr0
    cd ~

----
----
----


Zuerst werden einige Pakete mit pacman installiert:

    pacman -Sy vim git xorg xorg-xinit nitrogen picom base-devel firefox
                //vim -> texteditor
                //git -> GitLab software
                //xorg -> X : Grundlage für Grafische Fenster- und Videoanzeige
                //base-devel -> Paketesammlung zum kompilieren von Software z. B. für yay aber auch andere wichtige Entwicklerprogramme ( https://www.archlinux.org/groups/x86_64/base-devel/ )
                //nitrogen -> Wallpaper manager
                //picom -> Comoser
                //firefox -> ...

# Grafiktreiber je nach Grafikkarte:

    lspci -v | grep -A1 -e VGA -e 3D
    
Je nach verbauter Grafikkarte wird ein passender Grafiktreiber gewählt:

    Brand   Driver                  OpenGL          Documentation 
    AMD 	xf86-video-amdgpu 	mesa 	        https://wiki.archlinux.org/index.php/AMDGPU
    ATI     xf86-video-ati          mesa            https://wiki.archlinux.org/index.php/ATI
    Intel 	xf86-video-intel 	mesa 	        https://wiki.archlinux.org/index.php/Intel_graphics
    NVIDIA 	xf86-video-nouveau 	mesa 	        https://wiki.archlinux.org/index.php/Nouveau
    
Z. B. Installation für Intel:
    
    pacman -Sy xf86-video-intel mesa lib32-mesa
    
# Installation des AUR-Managers YAY per git clone:

    git clone https://aur.archlinux.org/yay-git.git
    
    chmod 700 yay-git
    
    cd yay-git
    
    makepkg -si
    
    
# 
    
    

