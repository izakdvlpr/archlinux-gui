# arch-gui
🖥 Arch Linux Guide

### Setps

1. Mudar teclado

```
loadkeys br-abnt2
```
Use o atalho `CTRL + W` e procure por `pt_BR.UTF-8` e de um `Enter`.
Depois use `CTRL + X` e digite `Y`, logo depois `Enter`.
```
nano /etc/locale.gen
locale-gen
export LANG=pt_BR.UTF-8
```

2. Verificar se temos acesso a internet.
```
ping -c3 archlinux.com
```

3. Conectar a internet.
```
iwctl
station list
station wlan0 get-networks
station wlan0 connect "<host_name>"
station wlan0 show
```
