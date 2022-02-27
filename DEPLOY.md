# App depolyment

MATLAB Compiler can be used to package code into standalone programs. PTB has not been tested for stand-alone use, and some changes to the PTB and Titta code was necessary for things to be properly compiled. But once these are merged, it should be possible to build without any further problems.

The list of folders that needed to be added to compile properly (these are settings that are passed to `mcc` on macOS):

```
-a ~/Code/Psychtoolbox-3/Psychtoolbox/PsychBasic/PsychPlugins/libptbdrawtext_ftg
l64.dylib ^
-a ~/Code/Psychtoolbox-3/Psychtoolbox/PsychOpenGL/MOGL/core ^
-a ~/Code/Psychtoolbox-3/Psychtoolbox/PsychOpenGL/PsychGLSLShaders ^
-a ~/Code/opticka/CoreProtocols ^
-a ~/Code/opticka/DefaultStateInfo.m ^
-a ~/Code/opticka/communication ^
-a ~/Code/opticka/tools ^
-a ~/Code/opticka/stimuli ^
-a ~/Code/opticka/ui/images ^
-a ~/Code/opticka/help ^
-a ~/Code/Titta/TittaMex/ ^
-R -startmsg,'Runtime Init START' ^
-R -completemsg,'Runime Init FINISH' ^
-d ~/build 
-m ~/Code/opticka/runOpticka.m
```

The runtime can be downloaded by the install packager or downloaded manually:

https://www.mathworks.com/help/compiler/install-the-matlab-runtime.html
https://www.mathworks.com/help/compiler/mcr-path-settings-for-run-time-deployment.html

## Command-line

Without starting MATLAB use the `mcc` command to build from Terminal (replace ^ with \ if you are using `bash` or `zsh`):

```
mcc -a ~/Code/Psychtoolbox-3/Psychtoolbox/PsychBasic/PsychPlugins/libptbdrawtext_ftg
l64.dylib ^
-a ~/Code/Psychtoolbox-3/Psychtoolbox/PsychOpenGL/MOGL/core ^
-a ~/Code/Psychtoolbox-3/Psychtoolbox/PsychOpenGL/PsychGLSLShaders ^
-a ~/Code/opticka/CoreProtocols ^
-a ~/Code/opticka/DefaultStateInfo.m ^
-a ~/Code/opticka/communication ^
-a ~/Code/opticka/tools ^
-a ~/Code/opticka/stimuli ^
-a ~/Code/opticka/ui/images ^
-a ~/Code/opticka/help ^
-a ~/Code/Titta/TittaMex/ ^
-R -startmsg,'Runtime Init START' ^
-R -completemsg,'Runime Init FINISH' ^
-d ~/build 
-m ~/Code/opticka/runOpticka.m
```
