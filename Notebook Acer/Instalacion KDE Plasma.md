# Entorno grafico

Instalar Xorg

```bash
sudo pacman -S xorg
```

Instalar Plasma

```bash
sudo pacman -S plasma-desktop
```

Instalar paquetes adicionales de plasma

```bash
sudo pacman -S plasma-nm plasma-pa powerdevil kscreen kdeplasma-addons kinfocenter breeze-gtk drkonqi kde-gtk-config kgamma5 khotkeys plasma-workspace-wallpapers sddm-kcm
```

Instalar Audio

```bash
sudo pacman -S pulseaudio-jack pulseaudio-equalizer
```

Instalar Paqueste extras

```bash
sudo pacman -S dolphin konsole kate ffmpegthumbs kdegraphics-thumbnailers packagekit-qt5
```

Habilitar SDDM

```bash
sudo systemctl enable sddm
```

Autologin SDDM

```
sudo mkdir /etc/sddm.conf.d
echo -e "[Autologin]\nUser=jp\nSession=plasma" | sudo tee -a /etc/sddm.conf.d/autologin.conf
```