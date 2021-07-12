Añadir Repositorio

```bash
sudo nano /etc/pacman.conf
```

```diff
+ Añadir las siguientes lineas
[archlinuxfr]
Server = http://repo.archlinux.fr/$arch
```

Actualizar repos

```bash
sudo pacman -Sy
```
Instalar YAY

```bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```