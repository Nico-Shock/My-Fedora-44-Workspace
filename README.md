[Hi :)](https://nico-shock.github.io/My-Fedora-44-Workspace/)

## General Info

- This setup was meant to be set up on a Fedora 44 everything headless installation without a desktop environment.

# Getting Ready

### Install dependencies and necessary tools

Dependencies:
```
sudo dnf in extra-cmake-modules qt5-qtbase-devel "kf5-*devel" "kf6-*devel" kdecoration-devel qt6-qtsvg-deve
```

Necessary tools:
```
sudo dnf in vim kernel-devel kernel-headers
```

### Make Download Faster
```
sudo vim /etc/dnf/dnf.conf
```
Add the following lines:
```
max_parallel_downloads=20
```

### Install additional packages

```
sudo dnf in -y flatpak git wget thermald ufw fzf python3 python3-pip fastfetch bluez blueman bluez-libs gnome-tweaks gedit
sudo systemctl enable --now ufw bluetooth
sudo ufw default deny
```

- Only install `gnome-tweaks` and `gedit` if you plan to install the Gnome desktop.
- If you want to use KDE Plasma, use these tools instead:
```
sudo dnf in kate 
```

Install Multimedia libraries:
```
sudo dnf group install multimedia
```

### Install Nvidia Drivers
```
sudo dnf in -y @base-x gcc make dkms acpid libglvnd-devel pkgconf xorg-x11-server-Xwayland libxcb egl-wayland akmod-nvidia xorg-x11-drv-nvidia-cuda --skip-broken --allowerasing
sudo reboot now
```

```
sudo su
```

```
echo "blacklist nouveau" >> /modprobe.d/blacklist.conf
echo "blacklist nova_core" >> /modprobe.d/blacklist.conf
```

```
echo "options nvidia NVreg_PreserveVideoMemoryAllocations=1" >> /modprobe.d/nvidia.conf
echo "options nvidia-drm modeset=1 fbdev=0" >> /modprobe.d/nvidia.conf
```

# General Steps

### Desktop Environment

Gnome:
```
sudo dnf in gnome-shell ptyxis gnome-control-center gnome-software gnome-menus gnome-shell-extensions gnome-system-monitor mutter gdm
sudo systemctl enable gdm
```

KDE Plasma:
```
sudo dnf in -y plasma-desktop konsole ark sddm dolphin
sudo systemctl enable sddm
```

- After installing your desktop environment, you need to switch to the graphical interface as default:
```
sudo systemctl set-default graphical.target
```

- After that, you can restart and log into your desktop:
```
sudo reboot now
```

- Now open your terminal and continue with the setup.

### Install the Terra Repo

```
sudo dnf in --nogpgcheck --repofrompath 'terra,https://repos.fyralabs.com/terra$releasever' terra-release
```

### Installing the CachyOS Kernel
```
sudo dnf copr enable bieszczaders/kernel-cachyos
sudo dnf in -y kernel-cachyos kernel-cachyos-devel-matched
sudo dnf copr enable bieszczaders/kernel-cachyos-addons
sudo dnf in -y cachyos-settings --allowerasing
sudo dracut -f --regenerate-all
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

### Installing Gaming Meta
```
sudo dnf in -y steam lutris wine bottles gamescope mangohud libva-utils vulkan-tools mesa-dri-drivers mesa-vulkan-drivers gamemode libadwaita wine-dxvk dxvk-native goverlay --skip-broken
sudo dnf copr enable atim/heroic-games-launcher
sudo dnf in -y heroic-games-launcher-bin
```

# Gnome Setup

## Extensions

- Blur My Shell  
- Just Perfection  
- Dash to Dock  
- Arc Menu (outdated) 
- Coverflow  
- Impatience  
- Gnome 4x UI Improvements  
- Caffeine   
- Open Bar  
- User Themes  
- System Monitor
- Extension List

## Extension Settings

### Blur My Shell:

- Default Rounded Pipeline: Radius 20, Brightness: 100  
- Panel: Dynamic: 30, Sigma: Brightness: 1.00  
- Applications: opaque focused windows: off, blur in overview: off, enable all by default: on, disable manually: browser

### Dash to Dock:

- Intelligent auto-hide: off  
- Size: 20
- Menu moved to the left  
- Show drives and devices: off  
- Minimize appearance: on  
- Window counter indicators: dots

### Just Perfection:

- Panel button spacing: 0px  
- Panel indicator spacing: 2px  
- Clock position: left + 1pt offset

### Open Bar:

#### Autotheming:
- Auto Refresh theme on background change: on
- Auto set bar margins and island bg alpha: off

#### Topbar properties:
- Type: Islands
- Apply in overview: on
- enable buttons proximity: off
- Bar Margins: 0
- Bar Height: 32

#### Dash/Dock:
- Style: Use top bar colors
- Disable Shadows
- Dock Size: 20

#### Gnome Shell:
- enable everything

Here are more of my custom configs:

- https://github.com/Nico-Shock/My-OpenBar-configs

## Commands

### Extensions Manager Installation:
```
flatpak install -y flathub com.mattjakeman.ExtensionManager
```

### MacOS Tahoe GTK Theme:
```
git clone https://github.com/vinceliuice/MacTahoe-gtk-theme.git --depth 1
cd MacTahoe-gtk-theme
./install.sh -l 
cd ..  
sudo rm -r MacTahoe-gtk-theme
```

### Rosepine Theme:
```
sudo dnf in -y gtk-murrine-engine
git clone https://github.com/Fausto-Korpsvart/Rose-Pine-GTK-Theme.git --depth 1
cd Rose-Pine-GTK-Theme/themes
./install.sh -c dark
cd ..
cd ..
sudo rm -r Rose-Pine-GTK-Theme
```

### Gnome Tweaks

- Set the Shell theme to `Adwaita`
- Set the other theme to the `Rosepine-Dark` theme.
- Set every font to the Inter Semi Bold one.

### Terminal (Ptyxis):
```
gsettings set 'org.gnome.Ptyxis.Profile:/org/gnome/Ptyxis/Profiles/'$PTYXIS_PROFILE'/' 'opacity' '0.70'
```

- Select DejaVu Sans Mono or Powerline font for the `Ptyxis` terminal.

# KDE Plasma Setup

## Install themes

### Material You Theme:

**Just follow the guide here:**

[KDE Material You Colors](https://github.com/luisbocanegra/kde-material-you-colors)

### Klassy Theme:

**Just follow the guide here:**

[Klassy](https://github.com/paulmcauley/klassy)

- In the Appearance settings, make sure to select the "Klassy" theme in the Window Decoration and the Application Style.

## Klassy Settings

**Buttons:**

- Set the icons to Fluent
- Set the icon size to 16
- Select bold icons
- Change the shape to full-height rounded rectangle

**Window:**

- Set the corner radius to 6
- Uncheck the box "Colourize with highlighted buttons colour"
- Go to the Window Outline Style and change the outline to 2.15, the opacity of active windows to 80% and of inactive windows to 55%
- Go to the title bar settings under title bar opacity and adjust the title bar opacity, the same way as the window rules opacity (or however you prefer). The opacity for active is 85% and for inactive 90% in my case.

### Klassy Application Style:

**General:**

- Uncheck every box with "Draw.." (so, uncheck all boxes)
- Under the transparency you can lower the transparency for your context menu.

## Konsole

### Change Font Size:

1. Right-click the `Konsole` window and select "Edit Current Profile," then go to Appearance
2. Here you can change the font, I like to change it to the DejaVu Sans Mono font (11px).

### Editing the Top Bar:

- Right-click the top bar of the `Konsole` and try to remove everything except the title bar

## Editing the Taskbar/Panel

1. Right-click on the Panel and go to edit mode
2. Disable the floating effect by changing the option to "Disabled." And enable the blur effect.
3. Change the taskbar size to 28.

# Setting up Transparency and Blur

## Make windows transparent

- Go to system settings -> window management -> window rules
- Add a new rule
- Under add property you can add 'active opacity' and 'inactive opacity', here you can set the value however you like. I set active to 85% and inactive to 90%.

### Make every window transparent

- In the new window rule set the window class to unimportant, and make sure you have set your opacity values.

### Make only specific windows transparent

- In the new window rule set your opacity values (and make sure to not change the window class)
- Then click the "Detect Window Properties" button and click on the window you want to make transparent.
- Then add every value that will pop up for the specific window that you picked.

## Blur everything

- Install [Better Blur DX](https://github.com/xarblu/kwin-effects-better-blur-dx)
- Follow the instructions in the repo for installation.
- Then go to system settings -> window management -> window effects

### Better blur settings

- Go to the settings in the window effects screen for Better Blur DX
- Under force blur settings select the option "blur everything except matching" and then check all the checkboxes (this will enable the blur properly for some reason).
- Under general settings set the blur strength to level 7 and the blur noise to level 2 or change it to whatever you prefer.

# General Setup

### Zsh:
```
sudo dnf in -y zsh
```
``` 
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/powerlevel10k  
echo 'source ~/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
```
```
chsh -s $(which zsh)
```

- In KDE Plasma with `Konsole` you need to right-click in the terminal window, then edit current profile and select the starting shell from /bin/bash to /bin/zsh)

## Fonts

### System Fonts

[Lora](https://fonts.google.com/specimen/Lora)

- I use the medium one

[Inter](https://fonts.google.com/specimen/Inter)

- I use the bold one
- This is my secondary font, I use the Lora font mainly

### Additional Fonts

```
git clone --depth 1 https://github.com/ryanoasis/nerd-fonts.git
cd nerd-fonts
./install.sh
cd ..
sudo rm -rf nerd-fonts
```
- This font will fix every icon, unicode and symbols.

```
sudo dnf in -y dejavu-sans-mono-fonts powerline-fonts
```
- The font I use for `Ptyxis` and `Konsole`

### Icons

- My primary icons are the **Tela Circle Icons**: go to the icon settings in Appearance in the KDE Plasma settings and click on get new, here you can search for every icon set
- Secondarily I like these fonts:
  - Papirus
  - win11-purple-dark
  - BeautySolar

### Mouse Cursor:

Mainly I use the **MacOS Tahoe** one, you need to manually install these: https://store.kde.org/p/2300466

- In your home directory create a new folder **".icons"**
- Put the **"MacOS-Tahoe-Cursor"** in the **.icons** folder
- Now in the appearance settings you can change your mouse cursor to this one

- Secondarily I also like other cursors:
  - Babita cursor
  - Breeze light cursor
  - Modern winxp cursor

## Browser Recommendations

- Helium:
```
sudo dnf in helium-browser-bin
```

- Brave
```
curl -fsS https://dl.brave.com/install.sh | sh
```

## Fastfetch

- I saved [this](https://github.com/Nico-Shock/Custom-Hyprland-Configs/blob/main/Fastfetch) config for it.
- To install you just need to create a config directory:
```
mkdir -p .config/fastfetch
```
- After that you need a config file
```
kate .config/fastfetch/config.jsonc
```
or
```
gedit .config/fastfetch/config.jsonc
```
- Just copy paste the content in that file and you're done.

## SDDM
