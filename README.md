# .dotfiles
This is my set of dotfiles for my arch linux setup, as well as some documentation for my own personal use. I use dracula color scheme with dracula pink as my main accent color. It's a simple setup, but it does what I need it to. Replicate at your own risk.

# Programs Used
* Window Manager: bspwm
* Hotkey Daemon: sxhkd
* System Bar: polybar
* Laucher: rofi
* Browser: firefox
* Terminal: kitty
* Editor: spacemacs

# Guide
## To Start
This guide presumes a base arch system, up to the point of having a functioning tty. I used the anarchy installer to automate most of the process and pre-configure a few things this time around, but I just installed a vanilla arch system. Remember to `pacman -Syyu` before anything else.

## Installation and Setup
1. Connect to wifi/ethernet. I used nmcli, so:
```
nmcli dev wifi connect "Network SSID" password "Password"
```
2. Install the base packages for the setup. Since it's arch,
```
sudo pacman -S git base-devel xorg bspwm sxhkd vim emacs kitty rofi sddm nitrogen firefox
```
3. Reboot, probably.
4. Copy the default configs for bspwm and sxhkd. The config directories need to exist first.
```
mkdir ~/.config
mkdir ~/.config/bspwm
mdkir ~/.config/sxhkd
cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm/
cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd/
```
5. Edit the configs so that rofi and kitty hotkeys will work. I use vim for this initial editing before I have spacemacs installed.
```
vim ~/.config/sxhkd/sxhkdrc
```
Then change the terminal emulator and program launcher to our installed programs launch.
```
# terminal emulator
super + Return
    kitty

# program launcher
super + space
    rofi -show drun
```
6. Enable SDDM and reboot.
```
sudo systemctl enable sddm
reboot
```
7. Login to the fresh bspwm and launch some terminals to check that things are working. At this point I've got a functioning wm/ui.
8. Install yay for AUR access. I'll use this for the rest of the software installation.
```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```
9. At this point I'm just gonna list a command to install the rest of the software I need/use. You may not need all of this stuff. If the configs are copied directly from this repo, you'll at least need: `kitty, discord, gnome-polkit, ksuperkey, picom, polybar, nerd-fonts-complete, ttf-ubuntu-font-family, maim, xclip`. Also worth noting I didn't use the AUR version of betterdiscord, I used the appimage from the main website. YMMV.
```
yay -S piper discord spotify btop gnome-polkit virtualbox thunar osu-lazer-bin opentabletdriver minecraft-technic-launcher surfshark-vpn qbittorrent tlp tlp-rdw acpi_call neofetch file-roller volman gvfs ksuperkey lxappearance dracula-icons-git ant-dracula-kvantum-theme dracula-gtk-theme spicetify-cli xf86-video-intel mpv picom polybar gthumb nerd-fonts-complete ttf-ubuntu-font-family maim xclip betterdiscord-installer
```
10. Install spacemacs. This one's easy, just run the following and then start emacs.
```
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
```
11. The base setup should be done now, the rest is just config, theming, and fixes!

## Config, Theming, and Tweaks
In no particular order. Most of this isn't necessary if the configs are copied directly from this repo.

### Config
* Set ksuperkey to autostart and bind rofi to `alt+F1`. I also set the `-kb-cancel` flag in my rofi keybind to `alt+F1,Escape` so that it also closes on super.
* To automatically start the apps I want on start, I make a `~/.config/bin/autostart.sh` script and bind it to `super + space`. This might be dumb but I prefer it.
* If dual booting, it's necessary to install `os-prober` if not installed, edit the grub config to use it, and then re-config grub.
* I set `gnome-polkit` to autostart in my bspwmrc, although I'm not sure I really need it since I'm not using pamac, but it occasionally gets called for some terminal commands if I forget to `sudo`.
* `tlp` and `tlp-rdw` need to be enabled and masked to work correctly. See https://linrunner.de/tlp/installation/index.html for instructions on that.
* XF86 keys need to be bound manually in sxhkdrc. I use `pactl` for audio and mic keys, and `xbacklight` for brightness.

### Theming
* Ubuntu and Ubuntu Mono are the fonts I use.
* Almost all my theme resources are taken straight from https://draculatheme.com with some tweaks to use dracula-pink as my main accent color.
* I use the dracula grub theme. Installed after the os-prober tweak to redo the grub config all in one go.
* I used `lxappearance` and the manager from `kvantum-gt5` to set their respective themes and icon theme, then edited .gtkrc-2.0 directly to set the font. This will be overwritten if the gtk theme is changed again.
* The wallpaper is a gradient generated from dracula-purple and dracula-pink.
* It's in my .spacemacs, but the dracula theme is a package that can be called to install and set to default in that file.
* The polybar theme is just a moderate modification on the example config with the colors copied from somewhere directly into the config file.
* The kitty theme I installed using the instructions from the dracula theme website mentioned above.
* I just use the `rofi` command as-is with some light color and size modification from the dracula theme website. No fancy scripts.
* For discord, there's a betterdiscord dracula theme and a plugin to change the font. I also use a few other plugins that I might list here in the future.
* For spotify, I use the dracula colorscheme from the Sleek theme, found here https://github.com/morpheusthewhite/spicetify-themes and installed using `spicetify-cli`.
* Changing the global font in firefox requires creating a userChrome.css, which I've included in this repo. More instructions can be found here https://www.userchrome.org/how-create-userchrome-css.html and I'm only using this to change the font but it's much more powerful. The rest of the theming is handled by the official dracula firefox theme and the font settings found within firefox itself.
* I use a modified version of the sugar-candy sddm theme, I've included my config for that.

### Tweaks
* It would be unreasonable to list all the tiny changes I've made to various configs, but I may try to do so in the future. There are a few notes though.
* OpenTabletDriver might require a fix to work, it did on a previous setup but not under the vanilla arch I used to create this set of dotfiles.
* For picom I just use a pared-down version of the example config. I just have it installed to fix screen tearing and an issue I had with java app dropdowns. No animations or anything fancy.
* Spicetify will require `chmod`ding wherever spotify is installed. Also worth noting I use the `--experimental-backend` flag when autostarting picom.
* As far as I can tell, btop only generates it's permanent config if some settings are changed and then the program is quit using the built-in menu.
* For firefox I use the regular ubuntu fonts because using the nerd-fonts caused some weird glyphs, but I use the nerd-fonts for everything else.

# Final Notes
* While I consider my setup mostly finished and completely functional, things are always a work in progress.
* I still haven't setup a lockscreen or power-manager, but I'm not sure if I strictly need to.
* Most of the stuff that people often use/make fancy menus for I do from the command line. Things like connecting to wifi, rebooting/shutting down, etc.
* I'm sure there's stuff I've missed here, but I'll be keeping this relatively up to date as a personal reference.
* My configs for things like grub and my sddm theme aren't included because they aren't in my home directory. If you want them, DM me on Reddit.
