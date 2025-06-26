- `apk` - `Android Package Kit`
- `sudo apt install adb`
### Android Debug Bridge
- to get an apk from your phone:
	- wireless/USB debugging ON
	- `adb pair HOST:PORT`
	- enter the pair code from your mobile device (you can find it in the wireless debugging setting)
	- `adb devices`
	- `adb shell pm list packages` - list all the packages in your mobile device.
		- use it with grep: `adb shell pm list packages | grep "bt"`
	- `adb shell pm path com.example.someapp` - get the full path of the package.
	- `adb pull /data/app/com.example.someapp-2.apk path/to/desired/destination` - get the the apk on your system.
### apktool
- [apktool github](https://github.com/iBotPeaches/Apktool)
- [apktool install docs](https://apktool.org/docs/install)

- increase the Java heap size using the `-Xmx` flag. (e.g. `-Xmx4G`, `-Xmx1024M`) 
![[java_heap_size_flags.png]]
- `apktool d <app>.apk` - decompile an apk file
- `apktool b source_dir` - build the apk 

- `.smali` - "**Smali files** are the human-readable version of **Dalvik bytecode**"
- [https://github.com/skylot/jadx] - Smali -> Java 

- [hacking android apps (for begginers)](https://www.youtube.com/watch?v=7kKl3nokZso)