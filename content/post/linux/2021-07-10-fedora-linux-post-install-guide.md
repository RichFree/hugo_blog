---
title: "Fedora Linux Post-Install Guide"
date: 2021-07-10T00:59:00+08:00
categories:
- linux
tags:
- guides
summary: Now that you have installed Fedora Linux, what's next?
draft: false

---

Here is the checklist to turn a vanilla Fedora set-up into something optimized
for my workflow:

- Install nvim 
- Accelerate dnf downloads
- Enable rpm fusion free and non-free repos
- Enable Flathub
- Install Timeshift for BTRFS snapshotting
- Install alacritty terminal emulator
- Change the shell to zsh and setup oh-my-zsh 
- Install GPU video acceleration packages
- Tweak Firefox
- Install Sway Window Manager
- Install applications for Sway

### Install nvim

Open gnome terminal and install nvim with the following:

```shell
$ sudo dnf install nvim
```

Copy the .vimrc file from my [dotfiles](https://github.com/RichFree/dotfiles) to
your home directory. 

Copy the nvim folder from my [dotfiles](https://github.com/RichFree/dotfiles) to
your .config directory. The init.vim files will guide nvim to use the .vimrc
found in the home directory.

### Accelerate dnf downloads

dnf is the fedora command for package management on Fedora Linux. 

```shell
$ sudo dnf check-update // check for update
$ sudo dnf upgrade --refresh // to check for update and upgrade packages
$ sudo dnf install <pkg> // to install packages
$ sudo dnf remove <pkg> // to remove packages
```

The default settings for dnf is slow. You can speed things up by enabling
default downloads in the dnf.conf file. 

```shell
$ sudo nvim /etc/dnf/dnf.conf
```

Add the following line to the bottom of the file:

```shell
max_parallel_downloads=10
fastestmirror=True
```
Now your dnf downloads should be much faster. You can perform a 

```shell
$ sudo dnf upgrade --refresh
```

### Enable RPM fusion free and non-free repos

Go to this [link](https://rpmfusion.org/Configuration) using the Firefox
browser. Install both the Free and NonFree repos for Fedora 34. 

### Enable Flathub

Go to this [link](https://flatpak.org/setup/Fedora/). Download and install the
"Flathub repository file". Your software installer application will now be
updated with the repos from flathub, enabling your to download flatpak
applications like Discord, Zoom, Skype, Steam etc. 

### Install Timeshift for BTRFS snapshotting

Using the Software application, install Timeshift. Run the application and go to
settings and choose BTRFS instead of RSync. Enable backups for daily, weekly,
and on boot. This ensures that you always have a recent snapshot to backup to.
Try making a backup, then install an application (obs?), then restore from the
snapshot you just made. This is to ensure that your system can indeed restore
from a snapshot and that your fedora install went well. 
If you system boots and your newly installed application (e.g. obs) cannot be
found, it means that you have successfully restored back to the snapshot. You
can open Timeshift again to verify that you indeed restored from a snapshot. 

Note that you can always delete a snapshot, even those that you just restored
from. 

### Install alacritty terminal emulator

```shell
$ sudo dnf install alacritty
```

Copy over my alacritty configs from my [dotfiles](https://github.com/RichFree/dotfiles)


### Change the shell to zsh and setup oh-my-zsh 

Install zsh

```shell
$ sudo dnf install zsh 
```

Change default shell

```shell
// change shell
$ grep <user> /etc/passwd
$ chsh -s $(which zsh)
$ grep <user> /etc/passwd
```

where \<user\> is your user. You can get your user by using 

```shell
$ whoami
```

Install oh-my-zsh

```shell
$ sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Install the following plugins for oh-my-zsh: 
```shell
# zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Configuring .zshrc: Just copy over my .zshrc from my [dotfiles](https://github.com/RichFree/dotfiles)
OR 
just include the following to your plugins:

```shell
plugins=(
	git
	dnf
	zsh-syntax-highlighting
	zsh-autosuggestions
)
```

then source your modified .zshrc file with 

```shell
source ~/.zshrc
```


### Install GPU video acceleration packages

Install intel-based packages for video acceleration. AMD users please do not do
this.

```shell
$ sudo dnf install ffmpeg libva libva-utils libva-intel-driver intel-gpu-tools
```

### Tweak Firefox

Open Firefox. Go to about:config. Change the following settings:

```shell
layers.acceleration.force-enabled = TRUE
gfx.webrender.all = TRUE
media.ffmpeg.vaapi-drm-display.enabled = TRUE
media.ffmpeg.vaapi.enabled = TRUE
media.rdd-process.enabled = FALSE
```

### Install Sway Window Manager

```shell
$ sudo dnf install sway waybar wofi
```

Copy over my ~/.config/{sway,waybar,wofi} folders from my
[dotfiles](https://github.com/RichFree/dotfiles) to your .config folder.

### Install applications for Sway

Install a polkit authenticator

```shell
$ sudo dnf install lxpolkit 
```

Install Insync (optional)

Go to [Insync](https://www.insynchq.com/) and follow the instructions for
Fedora 34. Note that this service requires payment and you can only link one
account permanently. However, it is the best linux file-sync service. 

Install Screenshot tools

```shell
$ sudo dnf install slurp grim
```

Brightness controls

```shell
$ sudo dnf install brightnessctl
```

Fonts

```shell
$ sudo dnf install jetbrains-mono-fonts-all
```

## Use Sway
Log out from Gnome. In the login screen, when entering your password, there is
an desktop selector on the bottom right. Choose sway, enter your password, and
be welcomed by a new Wayland-based tiling window manager.

Refer to my [guide](https://richardwong.xyz/linux/2021/07/10/sway-guide/) on how
to use the Sway Window Manager in the future.


