# Preconfiguracion

Cargar distribución de teclado latina

```bash
loadkeys la-latin1
```

Habilitar NTP

```bash
timedatectl set-ntp true
```

# Almacenamiento

Particionado

```
cfdisk /dev/sda
```

```diff
+ /dev/sda1 # 256M (EFI Partition) => /boot/efi
+ /dev/sda2 # (Linux) => /
+ /dev/sda3 # 2G (Linux Swap) => swap
+ /dev/sda4 # MAX (Linux) => /home
```

Formateado

```bash
mkfs.fat -F32 /dev/sda1
```

```bash
mkfs.ext4 /dev/sda2
```

```bash
mkswap /dev/sda3
swapon /dev/sda3
```

Montar la particion root

```bash
mount /dev/sda2 /mnt
```

Crear directorio para montar particion EFI

```bash
mkdir -p /mnt/boot/efi
```

Montar particion EFI

```bash
mount /dev/sda1 /mnt/boot/efi
```

Crear directorio para montar particion Home

```bash
mkdir -p /mnt/home
```

Montar particion Home

```bash
mount /dev/sda4 /mnt/home
```

Renombrar Home JP de instalacion anterior

```bash
mv /mnt/home/jp /mnt/home/jp_old
```

# Instalación

Crear directorio cache pacman

```bash
mkdir -p /mnt/var/cache/pacman/pkg
```

Montar Caché de Pacman

```bash
mount -t cifs -o username=pi "//192.168.1.103/datos/Software/Pacman Cache/pkg" /mnt/var/cache/pacman/pkg
```

Instalar paquetes Base y Kernel Linux

```bash
pacstrap /mnt base linux linux-firmware nano
```

# Configuración

Generar archivo de montado fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

Cambiar el root a la instalación

```bash
arch-chroot /mnt
```

Actualizar paquetes

```bash
pacman -Syu
```

Crear enlace de Zona Horaria Chilena

```bash
ln -sf /usr/share/zoneinfo/America/Santiago /etc/localtime
```

Actualizar reloj a Zona Horaria enlazada

```bash
hwclock --systohc
```

Configurar Hostname

```bash
nano /etc/hostname
```

```diff
+ Añadir lo siguiente al archivo:
jpacer
```

Generar idiomas

```bash
locale-gen
```

Añadir idioma

```bash
nano /etc/locale.conf
```

```diff
+ Añadir siguiente linea
LANG=es_CL.UTF-8
```

Configurar idioma

```bash
nano /etc/locale.gen
```

```diff
+ Descomentar la siguiente linea
#es_CL.UTF-8 UTF-8
```

Generar idiomas

```bash
locale-gen
```

Configurar idioma del teclado

```bash
nano /etc/vconsole.conf
```

```diff
+ Añadir siguiente linea
KEYMAP=la-latin1
```

Setear contraseña root

```bash
passwd
```

# Intel

Microcode Intel

```bash
pacman -S intel-ucode
```

# Grub

Instalar Grub

```bash
pacman -Sy grub efibootmgr
```

Instalar Boot loader

```bash
grub-install --target=x86_64-efi --bootloader-id='Arch Linux' --efi-directory=/boot/efi
```

Solucionar problema Intel Freeze

```bash
nano /etc/default/grub
```

```diff
+ Añadir lo siguiente a linea GRUB_CMDLINE_LINUX_DEFAULT
intel_idle.max_cstate=1
```

Generar configuración para Grub

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

# Paquetes adicionales

Driver wifi

```bash
pacman -S broadcom-wl-dkms
```

Paquetes adicionales

```bash
pacman -S networkmanager wireless_tools wpa_supplicant dialog base-devel linux-headers git net-tools sudo openssh
``````

Habilitar Network Manager

```bash
systemctl enable NetworkManager
```

Habilitar SSH

```bash
systemctl enable sshd
```

# Usuario

Agregar usuario

```bash
useradd -mG wheel jp
```

Contraseña Uusuario

```bash
passwd jp
```

Configurar sudo

```bash
nano /etc/sudoers
```

```diff
+ Descomentar siguiente linea
%wheel all=(all) all
```

Salir del chroot y reiniciar

```bash
exit
reboot
```
