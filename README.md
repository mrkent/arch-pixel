# arch-pixel

Automated installation script for arch linux for Chromebook Pixel

#### Resources
1. https://github.com/vitamins/archinstaller/
2. https://gist.github.com/thirteen37/5559107
3. https://bbs.archlinux.org/viewtopic.php?id=159912

## Instructions

### Preping the Pixel

##### Enter Dev Mode
1. Hold down Esc and Refresh while powering on the Pixel.
2. At the blank screen, press Ctrl-D.
3. You will see a prompt to enter dev mode. Press Enter.
4. Wait for the wipe. It will take about 5-10 minutes.
5. After the wipe, every reboot will show the white dev mode screen. Press Ctrl-D at this screen to boot to Chrome OS.

##### Enable USB and legacy boot
1. When in ChromeOS, start a terminal via `Ctrl-Alt-T`.
2. Type `shell` to get a real bash prompt.
3. Type `sudo crossystem dev_boot_legacy=1`
4. Type `dev_boot_usb=1` (This may not be required. Instead, pressing Esc at boot will allow you to choose boot media. The reason to avoid automatically booting from USB is that an SD card can be later placed in the Pixel permanently, increasing effective storage capacity. I am unaware of how to change this setting after ChromeOS is wiped.)

### Preping Arch and this script

##### Prerequisites 
1. Download the 2013.04.01 Arch ISO (https://mega.co.nz/#!htx1QLJK!07ap012cemdaaGOGeAT4geiLP5mXjkbhzYTERpVuHf8) (https://www.archlinux.org/releng/releases/2013.04.01/)
2. Install USB: `# dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx && sync`
3. Download this script: `curl -L https://github.com/mrkent/arch-pixel/tarball/master | tar xz`
4. Modify ari.conf as needed. Provided ari.conf.example works fine. It's best not to change anything that's unnecessary like adduser or passwd because it may cause script to abort.
5. It might be helpful to save this script onto 2nd USB drive, along with package list or config files you might installed before the first boot.


##### Installation

1. Reboot the Pixel with your Arch installation medium plugged in. To spare yourself the confusion, do not plug in any other drives.
2. At the white dev mode screen, press Ctrl-L to do a legacy boot.
3. Ignore the "Press Esc" and the BIOS will default to the Arch installer.
4. We will do a 64-bit install. Press Tab on the first installer option to edit the boot parameters.
5. Add ` mem=4G` to the end of the boot parameters and press Enter. Without this, the installer will fail to properly detect the amount of RAM and claim there's insufficient to load the kernel.
6. Set up internet: `iw dev` `ip link set wlp1s0 up` `wifi-menu wlp1s0`
7. Either download script or install 2nd USB and run this script as 'root' user

### Additional Packages
There are two possibilities to make the script install additional packages. You can add them to the packages array in the configuration file. Alternatively you can write the packages to `pkglist.txt`, one on each line. To generate a list of explicitly installed packages on an existing installation, use this command: `pacman -Qqent > pkglist.txt` You can also use both options at the same time, duplicated entries are eliminated by pacman.
Should one of the packages not be found in the repositories, e.g. if you have misspelled it, no packages are installed. Leave the "packages" array empty to skip this step.

### Required AUR packages, best to install them these immediately

1. https://aur.archlinux.org/packages/xf86-input-synaptics-mtpatch/
1. https://aur.archlinux.org/packages/touchegg/
