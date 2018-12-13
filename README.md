# Notes
Various notes on setting up our software + hardware environment. In general, all software development should take place under Linux, the default OS for our workstations. 

# Setting up Ubuntu 18.04
For Ubuntu, we are using the LTS version of Ubuntu as the default. For new installs the first thing to do is install Chinese text entry and language support: https://www.pinyinjoe.com/linux/ubuntu-18-gnome-chinese-setup.htm 

## Major Software:
1. Android Studio V3.2+ — https://developer.android.com/studio/ — used for the Mymou system. We will use both Java and Kotlin language support.
2. Visual Studio Code — great general purpose text editor, great Python support — https://code.visualstudio.com [download](https://code.visualstudio.com/docs/?dv=linux64_deb). Built in Git support etc. But other IDEs like [PyCharm](https://www.jetbrains.com/pycharm/) are also great.
3. Ultimaker Cura — for our Ultimaker 3 3D printer — https://ultimaker.com/en/products/ultimaker-cura-software 
4. Python 3 — I prefer to use [Anaconda](https://www.anaconda.com) to install and manage Python, personally I use [Miniconda](https://conda.io/docs/user-guide/install/index.html) to install a minimum then add use [conda](https://conda.io/docs/user-guide/tasks/manage-conda.html) to install packages as needed. It is important for each project to use different [environments](https://conda.io/docs/user-guide/tasks/manage-environments.html). 

## Problems
1. Android Studio cannot work without a VPN, it seems blocked. *How do Chinese developers solve this problem?*

# Languages

## Java / Kotlin
We will try to use Kotlin for all new code for Mymou, the automated behavioural test system. Currently Mymou is written in Java, but Kotlin and Java work well together. We may also use Kotlin for our backend database system.

## MATLAB + PTB
For desktop computers, [PTB](http://psychtoolbox.org) is still the best software for dedicated testing, especially for visual cognition tasks and all tasks requiring precise timing. To install PTB, you should use `git` to clone this fork: https://github.com/iandol/Psychtoolbox-3 — for Ubuntu AMD GPUs are better using the open source drivers. We will base new experiment code on this toolbox: https://github.com/iandol/opticka 

## Python 3
We will use Python for general purpose development, and for the Neural network training for Mymou and for [DeepLabCut](https://github.com/AlexEMG/DeepLabCut)
