# Software + Hardware Notes
Various notes on setting up our software + hardware environment. In general, all software development should take place under Ubuntu Linux, the default OS for our workstations.

# Setting up Ubuntu 18.04
For Ubuntu, we are using the LTS version of Ubuntu as the default. For new installs the first thing to do is install Chinese text entry and language support: https://www.pinyinjoe.com/linux/ubuntu-18-gnome-chinese-setup.htm — next you need to install the minimum number of tools for development. I always install at least: `sudo apt-get install build-essential vim curl file zsh git figlet jq libusb-1.0.0 freeglut3 libraw1394-11`. Once you have installed MATLAB, you can then try `sudo apt-get install matlab-support` for PTB compatibility.

# Installing Github repos
```
mkdir -p ~/Code
cd ~/Code
git clone --depth 1 https://github.com/iandol/Psychtoolbox-3.git
git clone https://github.com/iandol/opticka.git
git clone https://github.com/CogPlatform/Training.git
git clone https://github.com/CogPlatform/Titta.git
git clone https://github.com/CogPlatform/Mymou.git
```

## Major Software to Install:
1. MATLAB — latest version kept up-to-date.
1. PTB — use my custom fork and and install it using Git; then in MATLAB, `cd` to the install folder and run `SetupPsychtoolbox.m` directly.
1. For 18.04 install gamemode from https://launchpad.net/~samoilov-lex/+archive/ubuntu/gamemode -- for 19.10 it is available in apt already...
1. Tobii Pro Eye Tracker Manager – https://www.tobiipro.com/downloads/ 
1. Visual Studio Code — great general purpose text editor, great Python support — https://code.visualstudio.com [download](https://code.visualstudio.com/docs/?dv=linux64_deb). Built in Git support etc. But other IDEs like [PyCharm](https://www.jetbrains.com/pycharm/) are also great.
1. Android Studio V3.3+ — https://developer.android.com/studio/ — used for the Mymou system. We will use both Java and Kotlin language support, but all new code will use Kotlin alone.
1. Python 3 — I prefer to use [Miniconda](https://conda.io/docs/user-guide/install/index.html) to install then use [conda](https://conda.io/docs/user-guide/tasks/manage-conda.html) to install packages as needed. It is better for each project to use different [environments](https://conda.io/docs/user-guide/tasks/manage-environments.html). APT installed Python + PIP can break very easily, not sure why...
1. Ultimaker Cura — for our Ultimaker 3 3D printer — https://ultimaker.com/en/products/ultimaker-cura-software 

## Problems
1. Android Studio cannot always update SDK and other compnenets properly without a VPN, though sometimes it works OK.

# Languages used for Platform Software

## MATLAB + PTB
For desktop computers, [PTB](http://psychtoolbox.org) is still the best software for dedicated testing, especially for visual cognition tasks and all tasks requiring precise timing. To install PTB, you should use `git` to clone this fork: https://github.com/iandol/Psychtoolbox-3 — for Ubuntu AMD GPUs are better using the open source drivers. We will base new experiment code on this toolbox: https://github.com/iandol/opticka

## Kotlin / Java
We will aim to use Kotlin for all new code for Mymou, the automated behavioural test system. Currently Mymou is written in Java, but Kotlin and Java work well together. We may also use Kotlin for our main backend database system.

### Micronaut / KTor
If we use Kotlin for our backend, then lets investigate http://micronaut.io/ or https://ktor.io

### PostgreSQL or MongoDB
Each database should be suitable for our needs, and possibly if we use Micronaut, each service can use its own database.

## Python 3
We will use Python for general purpose development, and for the Neural network training for Mymou and for [DeepLabCut](https://github.com/AlexEMG/DeepLabCut).
