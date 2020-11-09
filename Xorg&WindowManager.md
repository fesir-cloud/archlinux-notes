# Xorg und Window Manager Installation:

Zuerst werden einige Pakete mit pacman installiert:

    pacman -Sy vim git xorg xorg-xinit nitrogen picom base-devel firefox
                //vim -> texteditor
                //git -> GitLab software
                //xorg -> X : Grundlage für Grafische Fenster- und Videoanzeige

# Grafiktreiber je nach Grafikkarte:

    lspci -v | grep -A1 -e VGA -e 3D
    
Je nach verbauter Grafikkarte wird ein passender Grafiktreiber gewählt:

    Brand   Driver              OpenGL      OpenGL(multilib)    Documentation 
    AMD 	xf86-video-amdgpu 	mesa 	lib32-mesa 	https://wiki.archlinux.org/index.php/AMDGPU
    ATI     xf86-video-ati          mesa    lib32-mesa      https://wiki.archlinux.org/index.php/ATI
    Intel 	xf86-video-intel 	mesa 	lib32-mesa 	https://wiki.archlinux.org/index.php/Intel_graphics
    NVIDIA 	xf86-video-nouveau 	mesa 	lib32-mesa 	https://wiki.archlinux.org/index.php/Nouveau
    
Z. B. Installation für Intel:
    
    pacman -Sy xf86-video-intel mesa lib32-mesa
    
# Installation des AUR-Managers YAY per git clone:

    git clone

