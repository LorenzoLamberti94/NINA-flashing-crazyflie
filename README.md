# Nina Flashing instructions (guidelines here: bitcraze website)

#Pull docker
$ docker pull bitcraze/aideck-nina

# Pull bitcraze repo
$ git clone https://github.com/bitcraze/aideck-gap8-examples

# Change to this commit (5 aug 2021)
$cd aideck-gap8-examples
$ git checkout a89e29072b7069b819dc930a8ca4a050b026d28d

# pull exact docker version of nina
docker pull bitcraze/aideck-nina:3.3.1

# Flash NINA:
$ cd NINA/firmware/


# Verify port of your olimex device : (should look like /dev/ttyUSB0 or another number, make sure to change the --device if your programmer is on another port)
$ dmesg

# Run now the docker command
> Working directory: AIdeck_examples/NINA/firmware
$ docker run --rm -it -v $PWD:/module/ --device /dev/ttyUSB0 --privileged -P bitcraze/aideck-nina /bin/bash -c "make clean; make menuconfig; make all; /openocd-esp32/bin/openocd -f interface/ftdi/olimex-arm-usb-ocd-h.cfg -f board/esp-wroom-32.cfg -c 'program_esp32 build/partitions_singleapp.bin 0x8000 verify' -c 'program_esp32 build/bootloader/bootloader.bin 0x1000 verify' -c 'program_esp32 build/ai-deck-jpeg-streamer-demo.bin 0x10000 verify reset exit'"

# specifying docker version/tag !
$ docker run --rm -it -v $PWD:/module/ --device /dev/ttyUSB0 --privileged -P bitcraze/aideck-nina:3.3.1 /bin/bash -c "make clean; make menuconfig; make all; /openocd-esp32/bin/openocd -f interface/ftdi/olimex-arm-usb-ocd-h.cfg -f board/esp-wroom-32.cfg -c 'program_esp32 build/partitions_singleapp.bin 0x8000 verify' -c 'program_esp32 build/bootloader/bootloader.bin 0x1000 verify' -c 'program_esp32 build/ai-deck-jpeg-streamer-demo.bin 0x10000 verify reset exit'"
#only flash
$ docker run --rm -it -v $PWD:/module/ --device /dev/ttyUSB0 --privileged -P bitcraze/aideck-nina:3.3.1 /bin/bash -c "/openocd-esp32/bin/openocd -f interface/ftdi/olimex-arm-usb-ocd-h.cfg -f board/esp-wroom-32.cfg -c 'program_esp32 build/partitions_singleapp.bin 0x8000 verify' -c 'program_esp32 build/bootloader/bootloader.bin 0x1000 verify' -c 'program_esp32 build/ai-deck-jpeg-streamer-demo.bin 0x10000 verify reset exit'"


# MENUCONFIG
in makeconfig you can set wither the drone in access point mode or passing through router.

# CHECK AI-deck IP
once flashed correctly go to your router page: 192.168.0.0
Check the espidf ip (dinamically assigned by the DHCP): in my case 192.168.0.244

you can show the same by running
nmap -sn 192.168.0.0/24

# Run viewer
now run python script with
	python viewer_custom.py -n 192.168.0.244
