## Raspberry Pi 4 Motorola Awtrix Lapdock Resolution fix

The Raspberry PI 4 with Raspberry OS (Bookworm) provides the wrong resolution in combination with the Lapdock from Motorola, resulting in an incorrect display.

![wrong resolution](imgs/faulty-resolution.jpg?raw=true "Wrong Resolution")

To fix this under Wayland (Raspberry Pi OS Bookworm) you must force the drm kms system to use a resolution of 1368x768@60 Hz. On the Raspberry Pi4 execute 


	$ sudo mkdir /usr/lib/firmware/edid
	$ wget https://github.com/bjoernh/Rpi4-motorola-Awtrix-lapdock/raw/refs/heads/main/1368x768_60.bin
	$ sudo mv 1368x768_60.bin /usr/lib/firmware/edid/

 Edit `/boot/firmware/cmdline.txt` and append this string to the end of the line

 	 video=HDMI-A-1:1368x768M@60D drm_kms_helper.edid_firmware=edid/1368x768_60.bin

Reboot the Pi and the Lapdock should now works with the Rapberry Pi 4 and Wayland :) 

![wrong resolution](imgs/working-resolution.jpg?raw=true "Wrong Resolution")

Tested with Rasberry OS 64bit Bookworm (build date November 19th 2024)

### Create EDID Date file by your own

If you don't trust the binary, you can create it by your own.


	$ sudo apt install zsh edid-decode automake dos2unix
	$ wget https://github.com/akatrevorjay/edid-generator/archive/master.zip
	$ unzip master.zip 
	$ cd edid-generator-master
	$ ./modeline2edid - <<< 'Modeline "1368x768_60"  85.86  1368 1440 1584 1800  768 769 772 795  -HSync +Vsync'
	$ make

This gives you an EDID binary file `1368x768_60.bin`.