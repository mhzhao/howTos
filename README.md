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
### FFMPEG related
- some vedio filter, `scale` to resize video, `eq` to adjust brightness, saturation etc., example:
`ffmpeg -y -i plotGraph.mp4 -vf scale=1920:-2,setsar=1:1,eq=brightness=0.05:saturation=2.0 plotGraphShrink.mp4`
one can use `ffplay` to preview the effect
### Hackintosh video card temperature and fan speed control, refer to [AMD Vega Power and Fan Control](https://www.tonymacx86.com/threads/guide-injection-of-amd-vega-power-and-fan-control-properties.267519/)
1. Install **VGTab** Utility, simply download and extract the zip file.
You need to **move** the App to a difference location (e.g. Desktop/) to use it. In case it can't generate data to desktop, try the following command `xattr VGTab_en.app` in the **Terminal**, if this doesn't work, then try `sudo xattr -r -d com.apple.quarantine VGTab_en.app`
2. Run **VGTab** utility and select the model type of your Vega card, modify the desired values (normally you can adjust the **target temperature**, if you want, you can also adjust the **Target speed** and **Idle speed**, do not adjust other values unless you know what you are doing), then click on the **Build PowerPlayTable** button and accept the notification to allow **VGTab** to save the generated files/data onto your MacOS desktop.
3. Use **GFXUTIL** to get your the device GFX0 ACPI identity. You can do this in **Terminal** by the following command `./gfxutil -f GFX0`, for example, I get mine 
> DevicePath = PciRoot(0x0)/Pci(0x1,0x1)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)
4. Mount your EFI partition (I use **Clover Configurator** to do this) and make a copy of your current config.plist (mine is located in /Volumes/EFI/EFI/CLOVER/config.plist). Then open config.plist, find the "**Devices**" section, and then move down to the "**Properties**" sub section. Below **Properties**, create a new Key for the PCI path of the Vega GPU
```
<key>PciRoot(0x0)/Pci(0x1,0x1)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)</key>
<dict> 

</dict>
``` 
  Inside the <key> </key> pair is the DevicePath you find in *3.*
  Go to the kext file you generate in *2.* (yours should be VGTab_XXXX.kext, here mine is VGTab_56.kext), in **Terminal** do  the following `cd ~/Desktop/VegaTab_56.kext/Contents` change the right kext name by your own, e.g. if you have             *VGTab_64.kext*, you should do `cd ~/Desktop/VegaTab_64.kext/Contents`. Open the file *Info.plist* under the directory, find **aty_properties** key, copy the contents inside <dict> </dict> below it, paste them into the <dict> </dict> you just created in the config.plist file. Save and reboot

### Gnuplot pdfcairo terminal font messy issue
It is caused by the pango 1.44 version, revert to version 1.43 for now to use
```
brew uninstall --ignore-dependencies pango
brew install iltommi/brews/pango
```

### pip install older python version site-packages

`pip freeze --path /usr/local/lib/python3.7/site-packages | awk -F = '{print $1}' | xargs -I{} pip install {}`

to upgrade all pip installed packages

`pip list --outdated | sed -n '3,$ p' | awk -F ' ' '{print $1}' | xargs su pip install --upgrade`

for the `pip` itself upgrade may cause errors, one needs to change the `which pip` to the correct version of `pip3 --version`, e.g. change `/usr/local/opt/python/libexec/bin/pip`

### manually enable kernels in jupyter
copy the kernel files to the appropriate locate, for example, for ROOT

`cp -r /usr/local/Cellar/root/6.22.00_1/etc/root/notebook/kernels/root /usr/local/share/jupyter/kernels`

### remove audio device (from MIDI Setup)

The driver should be in the "/Library/Audio/Plug-Ins/HAL" folder (It's the standard place for Core audio drivers)
e.g. delete MorphVOX device, remove driver (named SBVirtualMic.driver) if you after uninstall see MorphVOX Audio in Sound Settings
`sudo rm -rf /Library/Audio/Plug-Ins/HAL/SBVirtualMic.driver`

### ffmpeg writing Chinese subtitle problems

The problem showing `fontselect: failed to find any fallback for font: ` and `Glyph 0X7F8E not found, selecting one more font for` when trying to write Chinese subtitles, it's caused by libass latest version 0.15.1, go back to version 0.15.0 works, following this method to set back older version [How to install an older homebrew package](https://remarkablemark.org/blog/2017/02/03/install-brew-package-version/)

## Windows
#### 1. windows 10 home edition enable Hyper-V
save the following script to a file named: Hyper-V.cmd, 

pushd "%~dp0"

dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt

for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"

del hyper-v.txt

Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL

run the file as administator, at the end type **Y** to yes (note: do not shutdown the computer, the system will restart by itself), after restarting itself, you will see hyper-v related options under **Windows Administrative tools**
#### 2. figures in word look messy in wondows while look normal in Mac
When edited the word in Mac the figures show normal, but looks terrible (e.g. rotating 90 degree) when open the word in Windows. A better and easy way to fix this is to reformat the pdf figures by `pdfjam` or `pdfcrop` with landscape format.
`pdfjam` is very nice and easy program that can rearrange the figures and trim margins. Check [examples](https://github.com/DavidFirth/pdfjam). `pdfcrop` is a simple way to remove empty margins or set margins from each page in the input PDF file.
