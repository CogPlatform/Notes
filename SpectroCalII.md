# Installation

For Linux, to install the spectrocal serial interface edit `/etc/udev/rules.d/99-ftdi.rules` and add this line:

```
ACTION=="add", ATTRS{idVendor}=="0861", ATTRS{idProduct}=="1003", RUN+="/sbin/modprobe ftdi_sio" RUN+="/bin/sh -c 'echo 0861 1003 > /sys/bus/usb-serial/drivers/ftdi_sio/new_id'"
```

For macOS or Windows, see the CRS website: https://www.crsltd.com/tools-for-vision-science/light-measurement-display-calibation/spectrocal-mkii-spectroradiometer/nest/product-support#npm

# MALAB Control

```matlab
c = calibrateLuminance;
c.screen = 0; % screen to calibrate
c.nMeasures = 40; % number of measurements
c.port = '/dev/ttyUSB0'; % serial port to use

c.runAll();
```
