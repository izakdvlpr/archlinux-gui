# archlinux-gui

**Mudar teclado**

```bash
loadkeys br-abnt2
```

**Verificar acesso a internet**

```bash
ping -c3 archlinux.com
```

**Conectar a Rede Wi-fi**

```bash
iwctl
station list
station wlan0 get-networks
station wlan0 connect "<host_name>"
station wlan0 show
```

**Conectar a Rede via Usb Android Mobile**

```bash
ip link
dmesg | grep usb0
dhcpcd <device> # enp1s0f0u2
```

> Caso não funcionar tentar:

```
dhcpcd -d <device>
```

**Particionamento**

- SDA1 (Sistema EFI): 500M
- SDA2 (Sistema de Arquivos): 59,5G

<img src='https://i.imgur.com/0nDAUt9.png' />

```bash
cfdisk
```

**Formatando Partições**

```bash
lsblk
mkfs.ext4 /dev/root_partition
mkswap /dev/swap_partition
mkfs.fat -F 32 /dev/efi_system_partition
```

**Montando File Systems**

```bash
mount /dev/root_partition /mnt
mount --mkdir /dev/efi_system_partition /mnt/boot/efi
swapon /dev/swap_partition
```

**Atualizações de Mirror e Keyring**

```bash
reflector --latest 20 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
pacman -Syyy
pacman -S archlinux-keyring
```

**Instalando Sistema**

Se seu processador for Intel adicione o pacote `intel-ucode` ou se for da AMD adicione `amd-ucode`.

```bash
pacstrap /mnt base base-devel linux linux-firmware nano vim git
```

### Configurando Sistema

**Fstab**

```bash
genfstab -U -p /mnt >> /mnt/etc/fstab
```

**Chroot**

```bash
arch-chroot /mnt
```

**Time zone**

```bash
ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
hwclock --systohc --utc
date
```

**Localization**

```bash
nano /etc/locale.gen
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
export LANG=en_US.UTF-8
```

**Layout do Teclado**

```bash
echo KEYMAP="br-abnt2" > /etc/vconsole.conf
```

**Configurações de Rede**

```bash
echo "izakdvlpr" > /etc/hostname
```

**Configurando Usuário**

```bash
passwd
useradd -mg users -G wheel,storage,power -s /bin/bash archlinux
passwd archlinux
pacman -S sudo
visudo
```

<img src="https://i.imgur.com/YoSoRXl.png" />

**Bootloader**

```bash
pacman -S grub efibootmgr mtools os-prober
grub-install --target=x86_64-efi --bootloader-id=GRUB --recheck
grub-mkconfig -o /boot/grub/grub.cfg
```

**Gerenciador de Redes**

```bash
pacman -S networkmanager
systemctl enable NetworkManager
```

### Reboot

```bash
exit
shutdown now
```
### Post-installation

**zsh**

```bash
sudo pacman -S zsh
git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
chsh -s $(which zsh)
exit
```

**utils**

```bash
mkdir compilers
cd compilers
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
cd
paru
paru -S ranger neofetch cpufetch bashtop python-pip
pip install psutil
```

**configurar bspwm, sxhkd, polybar e kitty**

```bash
paru -S bspwm-rounded-corners sxhkd polybar kitty
mkdir ~/.config/{bspwm,sxhkd,polybar}
cp /usr/share/doc/polybar/examples/config.ini ~/.config/polybar
cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm
cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd
echo "polybar example &" >> ~/.config/bspwm/bspwmrc
vim ~/.config/sxhkd
# adicionar kitty
```

**xorg**

```bash
sudo pacman -S xorg xorg-xinit xterm xorg-xeyes xorg-xclock
```

> sudo pacman -S virtualbox-guest-utlis

**configurar x**

```bash
cp /etc/X11/xinit/xinitrc ~/.xinitrc
vim .xinitrc
```

```bash
xrandr --output Virtual1 --mode 1280x1024 &
setxkbmap -model abnt2 -layout br &
exec bspwm
```

**iniciar process x**

```bash
vim .zprofile
```

```bash
if [[ "$(tty)" = "/dev/tty1" ]]; then
  pgrep bspwm || startx "$XDG_CONFIG_HOME/X11/xinitrc"
fi
```
