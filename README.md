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
lsblk
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
```

5. Atualizações

```bash
reflector --latest 20 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
pacman -Syyy
pacman -S archlinux-keyring
```

6. Instalando Sistema

Se seu processador for Intel adicione o pacote `intel-ucode` ou se for da AMD adicione `amd-ucode`.

> No meu caso é da AMD.

```bash
pacstrap /mnt base base-devel linux linux-firmware linux-headers nano vim amd-ucode
```

7. Montando Partições

```bash
mkdir /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
genfstab -U /mnt >> /mnt/etc/fstab
```
