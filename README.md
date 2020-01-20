# howTos
all different software problems and solutions

#### 1. node-gyp couldn't install on MacOS catalina
the problem is that I could return nothing when do the following test, which it should be return things as showing
~ ❯❯❯ /usr/sbin/pkgutil --packages | grep CL
com.apple.pkg.CLTools_Executables
com.apple.pkg.CLTools_SDK_macOS1015
com.apple.pkg.CLTools_SDK_macOS1014
com.apple.pkg.CLTools_macOS_SDK
~ ❯❯❯
#### solutions: Uninstalling command-line tools as described in the [official documentation](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_CAN_I_UNINSTALL_THE_COMMAND_LINE_TOOLS_), then run xcode-select --install solves this problem for me
