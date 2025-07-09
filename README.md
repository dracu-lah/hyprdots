# Hyprland Setup Guide (Arch-based)

A straightforward guide to set up a functional and minimal Hyprland desktop environment on Arch-based systems.

## 1. Install AUR Helper (`yay`)

```bash
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay && makepkg -si
```

---

## 2. Install Hyprland and Core Packages

```bash
sudo pacman -S --needed \
  alacritty \
  bluez \
  blueman \
  brightnessctl \
  cliphist \
  fd \
  firefox \
  fzf \
  grim \
  hyprland \
  hyprpaper \
  hyprshot \
  kitty \
  ly \
  mpv \
  nemo \
  nwg-look \
  pipewire \
  pipewire-alsa \
  pipewire-audio \
  pipewire-jack \
  pipewire-pulse \
  playerctl \
  ripgrep \
  slurp \
  swaybg \
  swayidle \
  tmux \
  tlp \
  ttf-font-awesome \
  ttf-jetbrains-mono-nerd \
  waybar \
  wf-recorder \
  wireplumber \
  wl-clipboard \
  wofi \
  xdg-desktop-portal-hyprland \
  stow
```

---

## 3. Install Additional AUR Packages

```bash
yay -S waylogout-git neovim-git wifi-qr
```

---

## 4. Power Management

### Enable TLP

```bash
sudo systemctl enable --now tlp.service
```

### Auto Suspend on Low Battery (5%)

Create a rule:

```bash
sudo nano /etc/udev/rules.d/99-lowbat.rules
```

Paste:

```bash
SUBSYSTEM=="power_supply", ATTR{status}=="Discharging", ATTR{capacity}=="[0-5]", RUN+="/usr/bin/systemctl suspend"
```

---

## 5. QEMU/KVM Virtualization Setup

Reference: [christitus.com](https://christitus.com/setup-qemu-in-archlinux/)

### Install Required Packages

```bash
sudo pacman -S --needed \
  qemu \
  virt-manager \
  virt-viewer \
  dnsmasq \
  vde2 \
  bridge-utils \
  openbsd-netcat \
  ebtables \
  iptables \
  libguestfs
```

### Configure libvirt

Edit config:

```bash
sudo nano /etc/libvirt/libvirtd.conf
```

Set:

```conf
unix_sock_group = "libvirt"
unix_sock_rw_perms = "0770"
```

Add your user:

```bash
sudo usermod -aG libvirt $(whoami)
newgrp libvirt
```

Reboot, then run:

```bash
virt-manager
```

---

## 6. Apply Dotfiles

Clone and apply using `stow`:

```bash
git clone https://github.com/dracu-lah/hyprdots ~/hyprdots
cd ~/hyprdots
stow .
```
