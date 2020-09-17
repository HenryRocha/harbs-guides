## Reintall Manjaro on a dual boot configuration with UEFI

To reinstall simply do not format the UEFI partition and the other OS's partions. GRUB will handle the new install.

## Fix A2DP Sink not available for KDE on Manjaro

Edit the file in `/etc/bluetooth/main.conf` and set `AutoEnable=true`.

## Using Docker to run Quartus Prime for FPGA development

Use the tools privided by this amazing [repo][1]. That will get Quartus running already but to program the FPGA we need to pass the USB devices to the container, along with the correct permissions.

First, on the host machine, create a new file in `/etc/udev/rules.d/` called `51-altera-usb-blaster.rules` and add the following to the file:
```
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6001", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6002", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6003", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6010", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6810", MODE="0666"
```
_For more information on why this is needed go [here][2]_

After that we need to change the last few lines of the `run-fpga.sh` script used by the [repo][1] to look like this:
```
docker run --rm --privileged -it \
-u $(id -u):$(id -g) \
-e DISPLAY $x11flags $ethernet \
-h $tool-$(whoami) \
-v /dev/bus/usb/:/dev/bus/usb/ \
-v $docker_home:/home/$(whoami) \
$tag $*
```

Links for later reference:
- [Example of docker script for another FPGA docker repo](https://github.com/Gekkio/docker-fpga/blob/master/lattice-diamond/run.sh)
- [Dockerized FPGA toolchains](https://section5.ch/index.php/2017/01/20/669/)
- [How to containerize an FPGA Application?](https://www.accelize.com/how-to-containerize-an-fpga-application/)
- [Intel Quartus Prime in Docker](https://www.jamieiles.com/posts/quartus-docker/)
- [Arck Wiki: Altera Design Software](https://wiki.archlinux.org/index.php/Altera_Design_Software)

[1]: https://github.com/halfmanhalftaco/fpga-docker "FPGA-Docker"
[2]: https://wiki.archlinux.org/index.php/Altera_Design_Software#USB-Blaster_Download_Cable_Driver "Arch Wiki: USB-Blaster_Download_Cable_Driver"
