# How to set up TWO displays for PTB + Ubuntu

PTB uses and optimised display mode where X-Windows is set up so MATLAB occupies one X-screen and PTB can fully control another X-screen (it is not used by Ubuntu GUI at all). The important point is that this system uses X11 config files, and so you should first ensure that X!! doesn't have lots of other config files that may interfere. The config files are stored in `/etc/X11/` and `/etc/X11/xorg.conf.d/`. Check to see if the is an `xorg.cong` file in `/etc/X11/` or other files in `/etc/X11/xorg.conf.d/`.

Make sure you have properly installed MATLAB + PTB:

1) Don't use `sudo` to install MATLAB, which means don't select the option to install executable shortcuts in the installer.
2) After MATLAB is installed, make sure you run `sudo apt-get install matlab-support` and follow instructions, and replace the C++ libraries.
3) Get PTB (I prefer to use `git` so I can keep PTB up-to-date). Make sure it is installed in a `~/` home folder (I use `~/Code/Psychtoolbox`).
4) `cd` to the PTB folder and run `SetupPsychtoolbox.m` — make sure it runs properly and you see no errors.

Once installed and tested (see if `VBLSyncTest.m` runs etc.), make sure your TWO monitors are plugged in and working (Ubuntu extends the display by default):

1) Run `XOrgConfCreator` and select the multi-monitor option. 
2) Unless you know what you are doing, don't change any advanced settings yet.
2) Save the config using the default options (PTB names the files for you and puts them in the right place).
2) `XOrgConfSelector.m` to choose that file.
3) Log out and log back in to "activate" this configuration file.

You can also use `XOrgConfSelector.m` to remove the config file (select **0** as an option). Or you can manually delete the file yourself if Ubuntu cannot login porperly then you need to use a console to manually delete the file:

1) Type CTRL+ALT+F3 to enter a terminal session.
2) log in with user name and password.
3) Type `sudo rm /etc/X11/xorg.conf.d/90-ptbconfig.conf`
4) Try CTRL+ALT+F1 to switch back to the GUI, or reboot otherwise.

