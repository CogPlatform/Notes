# App depolyment

MATLAB Compiler can be used to package code into standalone programs. PTB has not been tested for stand-alone use, and some changes to the PTB and Titta code was necessary for things to be properly compiled. But once these are merged, it should be possible to build without any further problems.

The list of folders that needed to be added to compile properly (these are settings that are passed to `mcc`:

```
-a /Users/ian/Code/Psychtoolbox-3/Psychtoolbox/PsychBasic/PsychPlugins/libptbdrawtext_ftgl64.dylib
-a /Users/ian/Code/Psychtoolbox-3/Psychtoolbox/PsychOpenGL/MOGL/core
-a /Users/ian/Code/Psychtoolbox-3/Psychtoolbox/PsychOpenGL/PsychGLSLShaders
-a /Users/ian/Code/opticka/CoreProtocols
-a /Users/ian/Code/opticka/DefaultStateInfo.m
-a /Users/ian/Code/opticka/communication
-a /Users/ian/Code/opticka/tools
-a /Users/ian/Code/opticka/stimuli
-a /Users/ian/Code/opticka/ui/images
-R -startmsg,'RUNTIME INIT START'
-R -completemsg,'RUNTIME INIT END'
-v
```

The runtime can be downloaded by the install packager or downloaded manually:

https://www.mathworks.com/help/compiler/install-the-matlab-runtime.html
https://www.mathworks.com/help/compiler/mcr-path-settings-for-run-time-deployment.html

