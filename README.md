# ESP HomeKit Vagrant Build Environment

In order to run [maximkulkin/esp-homekit-demo](https://github.com/maximkulkin/esp-homekit-demo) you need [pfalcon/esp-open-sdk](https://github.com/pfalcon/esp-open-sdk) which requires a case-sensitive build environment on OS X and is just generally a pain.

## Using

1. Edit the `Vagrantfile` and update the USB device identification section for your ESP. `VBoxManage list usbhost` on the host machine will help you identify the device. (N.B. feel free to edit CPU/memory allocations while you're here, but the initial provisioning will fail if set to the default 512MB - I did not test the actual minimum this will build at)
2. `vagrant up` and wait
3. `vagrant ssh` to connect to the build environment
4. To test esp-homekit-demo, `cd esp-homekit-demo`
5. `cp wifi.h.sample wifi.h` and edit `wifi.h`
6. Execute `make -C examples/led test` and if all goes well it should compile and write to your device
7. This directory should be visible and usable from `/vagrant/` with all the tools you expect for compiling and burning ESP firmware

## Notes

- The Vagrantfile assumes that your ESP is using the cp210x serial UART driver
- The ESPPORT variable is set to point to /dev/TTYUSB0 which may need to be adjusted depending on where your USB serial device is detected within the VM
