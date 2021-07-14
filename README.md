
# Some fixes for running Linux as a daily driver on Acer Chromebook 14 CB3-431
**Update Log:**

 - 14/07/2021 - Updated Keyboard fix for Ubuntu/Debian distros
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
All thanks to [J. Starnes @ AskUbuntu](https://askubuntu.com/questions/974073/no-audio-on-acer-chromebook-14-under-ubuntu-17-10) for the fix. [MrChromebox](https://www.reddit.com/r/chrultrabook/comments/hji0sv/fix_for_audio_on_ubuntu_distros_2004/fwmd2h2?utm_source=share&utm_medium=web2x) states that "this fix should work for all Braswell ChromeOS devices, except maybe the R11/C738T, since it uses a different audio codec/amp"

### Installation
### 1. Fixing Audio via Speakers
1. Open a terminal and enter the following commands line-by-line:
```bash
wget https://raw.githubusercontent.com/rgvxsthi/Braswell-EDGAR-Linux-Fixes/master/asound.state
pgrep alsa
sudo cp asound.state /var/lib/alsa/asound.state
sudo alsa force-reload
````
3. Test to check that your sound is working or not (via Sound Mixer/Firefox etc)
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


## Keyboard

All thanks to [TickleMeElmo132 on Reddit](https://www.reddit.com/user/TickleMeElmo132) for the [great write-up on this very easy fix](https://www.removeddit.com/r/GalliumOS/comments/nx25tq/install_chromebook_keyboard_on_ubuntu_and_debian/). 

To use the keys the same way they work under ChromeOS on your Ubuntu-based distro, follow the below steps.
### Installation
1. Open up a terminal and type the following commands:
```bash
sudo apt install intltool xutils-dev -y
```
2. Once the above build utilities have installed/updated, open up a terminal to download GalliumOS's default keyboard configuration. This file is hosted on GalliumOS's build server and also mirror on this GitHub repository for easy access/download:
```bash
wget https://apt.galliumos.org/pool/main/x/xkeyboard-config/xkb-data-i18n_2.23.1-1ubuntu1-galliumos1_all.deb
```
3. Once the file has downloaded, install it with terminal:
```bash
sudo dpkg -i xkb-data-i18n_2.23.1-1ubuntu1-galliumos1_all.deb
```
4. Restart your computer and check your keyboard layout settings. You should have a new listing called "Chromebook". Play around with the settings and pick the option that suits best:
5. 
![enter image description here](https://image.prntscr.com/image/loQ52BtPQDWN28I6U4su0g.png)


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
