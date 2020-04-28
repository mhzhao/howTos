# howTos
all different software problems and solutions

## MAC and Linux
#### 1. node-gyp couldn't install on MacOS catalina
the problem is that I could return nothing when do the following test, which it should be return things as showing
~ ❯❯❯ /usr/sbin/pkgutil --packages | grep CL
com.apple.pkg.CLTools_Executables
com.apple.pkg.CLTools_SDK_macOS1015
com.apple.pkg.CLTools_SDK_macOS1014
com.apple.pkg.CLTools_macOS_SDK
~ ❯❯❯
#### solutions: Uninstalling command-line tools as described in the [official documentation](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_CAN_I_UNINSTALL_THE_COMMAND_LINE_TOOLS_), then run xcode-select --install solves this problem for me

## Windows
#### 1. windows 10 home edition enable Hyper-V
save the following script to a file named: Hyper-V.cmd, run the file as administator, at the end type **Y** to yes (note: do not shutdown the computer, the system will restart by itself), after restarting itself, you will see hyper-v related options under **Windows tools**
