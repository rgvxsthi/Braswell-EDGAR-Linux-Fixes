# Some fixes for running Linux as a daily driver on Acer Chromebook 14 CB3-431
## (Edgar // Braswell architecture)

# Fixes for Arch-based distros
Tested on EndeavourOS XFCE on an Acer Chromebook 14 CB3-431

## Update mirror list
The below command updates the Pacman mirrorlist with latest 200 updated HTTP and HTTPS mirrors (configured for AU and NZ regions - refer to [ArchWiki](https://wiki.archlinux.org/index.php/Mirrors) for more info) sorted by download speed. The following command uses [reflector](https://wiki.archlinux.org/index.php/Reflector) to update and save the contents to /etc/pacman.d/mirrorlist.
1. Open a Terminal window
2. Run the below command:
```bash
reflector --latest 200 --country AU --country NZ --protocol http --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```


# Fixes for Ubuntu-based Distros (Ubuntu/Xubuntu/Kubuntu/Mint etc...)
Most of these fixes worked properly for me under Kubuntu 20.04 and KDE neon 5.18 on an Acer Chromebook 14 CB3-431! (seems to be good on most Ubuntu-based distros)

## Audio
All thanks to [J. Starnes @ AskUbuntu](https://askubuntu.com/questions/974073/no-audio-on-acer-chromebook-14-under-ubuntu-17-10) for the fix. 

### Installation
### 1. Fixing Audio via Speakers
1. Open a terminal and enter the following commands line-by-line:
```bash
wget https://raw.githubusercontent.com/rgvxsthi/Braswell-EDGAR-Linux-Fixes/master/asound.state
pgrep alsa
sudo cp asound.state /var/lib/alsa/asound.state
sudo alsa force-reload
````
3. Test to check that your sound is working or not (via Sound Mixer/Firefox etc)/
4. If working, use the following commands to make the changes permanent
```bash
alsactl init
sudo alsactl store --file /var/lib/alsa/asound.state
sudo alsa force-reload
```
### 2. Fixing Audio for Headphones
Once you have followed the above steps and your sound is working (check after a reboot as well), you have to follow the following steps to unmute your headphones.
1. Open a terminal and type
```bash
alsamixer
```
2. Press F6 (brightness decrease key)
3. Select your sound card (chtrt5650 for me)
4. Press right arrow key once to move to Headphone Channel
5. Press "m" to unmute it and the ESC key to save these changes.


## Keyboard (multimedia keys - top row)
All thanks to [optio50](https://github.com/optio50/ChromeBook-Keyboard-xkb) for the fix. 
Your basic keyboard keys should already be working good other than the exception of the top multimedia keys. To use the keys the same way they work under ChromeOS on your Ubuntu-based distro, follow the below steps.
### Installation
1. Navigate to /usr/share/X11 folder.
2. Rename the "xkb" folder to "xkb.old"
3. Download the xkb.zip file from [here](https://github.com/optio50/ChromeBook-Keyboard-xkb/raw/master/xkb.zip) and unzip in the same folder i.e. /usr/share/X11.
4. Reboot your laptop.
5. In your distro's keyboard settings, you should have new settings for Chromebook (Google - Chromebooks).

## Fix for boot-menu not opening/boot options not displaying and only showing a black screen
If for some reason, the boot options/menu show a black screen and you're not able to boot from the USB, try the following fix (big ups to MrChromebox for the [fix](https://www.reddit.com/r/coreboot/comments/fwl6wv/black_screen_when_selecting_boot_menu_or_boot/)). These should work on any distro, not just Ubuntu-based.
1. Boot to your installed linux distro 
2. In Terminal, type the following command:
```bash
sudo efibootmgr -O
```
(capital O and not a zero)

3. Reboot and you should be able to see the boot menu/options now.


P.S. if you're able to see the EFI shell, the following command might also work (haven't tested):
```bash
setvar BootOrder =
```
and then type
```bash
exit
```
Reboot and you should be able to see the boot menu/options now.
