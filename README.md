# arch-gui
🖥 Arch Linux Guide

### Setps

1. Mudar teclado

```bash
loadkeys br-abnt2
```
Use o atalho `CTRL + W` e procure por `pt_BR.UTF-8` e de um `Enter`.
Depois use `CTRL + X` e digite `Y`, logo depois `Enter`.
```bash
nano /etc/locale.gen
locale-gen
export LANG=pt_BR.UTF-8
```

2. Verificar se temos acesso a internet.

```bash
ping -c3 archlinux.com
```

3. WI-FI

```bash
iwctl
station list
station wlan0 get-networks
station wlan0 connect "<host_name>"
station wlan0 show
```

4. Particionamento

Neste tutorial vamos utilizar um SDD de exemplo. 

- SDA1 (Sistema EFI): 500M
- SDA2 (Sistema de Arquivos): 59,5G

<img src='https://i.imgur.com/0nDAUt9.png' />

```bash
cfdisk
```

5. Formatando Partições

```bash
lsblk
mkfs.ext4 /dev/root_partition
mkfs.fat -F 32 /dev/efi_system_partition
```

6. Montando File Systems

```bash
mount /dev/root_partition /mnt
mount --mkdir /dev/efi_system_partition /mnt/boot/efi
```

7. Atualizações

```bash
reflector --latest 20 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
pacman -Syyy
pacman -S archlinux-keyring
```

8. Instalando Sistema

Se seu processador for Intel adicione o pacote `intel-ucode` ou se for da AMD adicione `amd-ucode`.

```bash
pacstrap /mnt base base-devel linux linux-firmware
```

9. Configurando Sistema

```bash
genfstab -U -p /mnt >> /mnt/etc/fstab
arch-chroot /mnt
echo "izakdvlpr" > /etc/hostname
```

10. Gerando Locale

```bash
nano /etc/locale.gen
locale-gen
echo LANG=pt_BR.UTF-8 > /etc/locale.conf
export LANG=pt_BR.UTF-8
```

11. Layout do Teclado

```bash
echo KEYMAP="br-abnt2" > /etc/vconsole.conf
```

12. Fuso Horário

```bash
ln -s /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
hwclock --systohc --utc
date
```

13. Configurando User

```bash
passwd
useradd -mg users -G wheel,storage,power -s /bin/bash izak
passwd izak
pacman -S sudo
visudo
```

<img src="https://i.imgur.com/YoSoRXl.png" />

14. Bootloader

```bash
pacman -S grub efibootmgr mtools
grub-install --target=x86_64-efi --bootloader-id=GRUB --recheck
grub-mkconfig -o /boot/grub/grub.cfg
```

15. Gerenciador de Redes

```bash
pacman -S networkmanager
systemctl enable NetworkManager
```
