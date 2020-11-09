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
                //base-devel -> Paketesammlung von 24 wichtigen Entwicklerprogrammen ( https://www.archlinux.org/groups/x86_64/base-devel/ ) zum Beispiel grep zum suchen oder make zum Kompilieren von Software z. B. hier für YAYbenutzt
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
    
    makepkg -si     //Mehrmals mit Y bestätigen
    
Jetzt ist YAY installiert.
    
# Installation von dwm (WindowManager) von Distrotubes AUR:

    yay -S dwm-distrotube-git st-distrotube-git dmenu-distrotube-git
    
    // Ein par mal mit enter bestätigen und userpasswortabfragen beantworten.
    
    
    
# Xinit Setup:

    cp /etc/X11/xinit/xinitrc /home/chyr0/.xinitrc      //Wichtig ist der Punkt vor dem 2. xinitrc
    
    nano /home/chyr0/.xinitrc
       
        
        >
        >
        >
        >
        >
