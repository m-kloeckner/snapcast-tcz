# snapcast-tcz
-----

A PiCore Snapcast extension for the RaspberryPi.

This tcz extension allows to run the Snapcast's client component in a PiCore installation.


## Installation
You can either (statically) build snapclient and wrap the provided file structure in a squashfs package by yourself, or download the prebuild package.

### Build snapclient
On a Raspbian powerd Pi, run the following to install the build dependencies:

    $ sudo apt-get install git build-essential
    $ sudo apt-get install libboost-dev libboost-system-dev libboost-program-options-dev libasound2-dev libvorbis-dev libflac-dev alsa-utils libavahi-client-dev avahi-daemon squashfs-tools

Download/git clone the latest source from the Snapcast Github repository and replace the 'Makefile' within the 'snapclient' folder with the 'Makefile' provided here.

Then run:

    $ sudo make snapclient

Download/git clone the 'tcz' folder structure provided here, replace the 'snapclient' binary in 'tcz/usr/sbin/' with the binary you compiled yourself and wrap the complete 'tcz' folder with the following command:

    $ mksquashfs tcz snapclient.tcz
    
Then follow the steps in 'Prebuild'.

## Prebuild
Download the 'snapclient.tcz' extension provided here and transfer it to your PiCore extension.

    $ scp snapclient.tcz tc@IP_ADDRESS:/tmp
    
SSH into your PiCore installation (User: tc, Password: PiCore) and run the following commands:

    $ mv /tmp/snapclient.tcz /mnt/mmcblk0p2/tce/optional/
    $ echo "snapclient.tcz" >> /mnt/mmcblk0p2/tce/onboot.lst
    $ echo "usr/local/etc/init.d/snapclient" >> /opt/.filetool.lst
    $ echo "/usr/local/etc/init.d/snapclient start" >> /opt/bootlocal.sh
    $ tce-load -wi libvorbis
    $ tce-load -wi libogg
    $ tce-load -wi flac
    $ tce-load -wi avahi
    $ sudo filetool.sh -b
    $ sudo reboot
    
Voilà! Play some music via Snapserver and the music will be played through your PiCore player.

## Todo

- [ ] Improve init script 
- [ ] Allow snapclient configuration via PiCore web-interface
- [ ] Resolve dependency issues on PiCore and do not compile statically
