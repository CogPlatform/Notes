# Configuring Touchscreens with 2 X-Screens

https://wiki.archlinux.org/index.php/Touchscreen
https://www.freedesktop.org/wiki/Software/xinput_calibrator/
https://github.com/tias/xinput_calibrator
https://github.com/reinderien/xcalibrate



```
less /proc/bus/input/devices
```

```
[I] ➜ DISPLAY=:0.0 xrandr
Screen 0: minimum 320 x 200, current 5120 x 2880, maximum 16384 x 16384
DisplayPort-0 connected primary 5120x2880+0+0 (normal left inverted right x axis y axis) 527mm x 296mm
[I] ➜ DISPLAY=:0.1 xrandr                                 
Screen 1: minimum 320 x 200, current 1920 x 1080, maximum 16384 x 16384
DisplayPort-3 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 377mm x 212mm
```

```
[I] ➜ xinput             
⎡ Virtual core pointer                    	id=2	[master pointer  (3)]
⎜   ↳ Virtual core XTEST pointer              	id=4	[slave  pointer  (2)]
⎜   ↳ M585/M590 Mouse                         	id=17	[slave  pointer  (2)]
⎜   ↳ ILITEK ILITEK-TP Mouse                  	id=9	[slave  pointer  (2)]
⎜   ↳ ILITEK ILITEK-TP                        	id=8	[slave  pointer  (2)]
⎜   ↳ PixArt Dell MS116 USB Optical Mouse     	id=13	[slave  pointer  (2)]
```


```
[I] ➜ DISPLAY=:0.1 xinput_calibrator --misclick 0 --device "ILITEK ILITEK-TP" 
Calibrating standard Xorg driver "ILITEK ILITEK-TP"
	current calibration values: min_x=0, max_x=65535 and min_y=0, max_y=65535
	If these values are estimated wrong, either supply it manually with the --precalib option, or run the 'get_precalib.sh' script to automatically get it (through HAL).
	--> Making the calibration permanent <--
  copy the snippet below into '/etc/X11/xorg.conf.d/99-calibration.conf' (/usr/share/X11/xorg.conf.d/ in some distro's)
Section "InputClass"
	Identifier	"calibration"
	MatchProduct	"ILITEK ILITEK-TP"
	Option	"MinX"	"6465"
	Option	"MaxX"	"73935"
	Option	"MinY"	"16096"
	Option	"MaxY"	"72528"
	Option	"SwapXY"	"0" # unless it was already set to 1
	Option	"InvertX"	"0"  # unless it was already set
	Option	"InvertY"	"0"  # unless it was already set
EndSection
```





https://wiki.archlinux.org/index.php/Talk:Calibrating_Touchscreen#Libinput_breaks_xinput_calibrator
