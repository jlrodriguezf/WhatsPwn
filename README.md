# Description
Linux tool used to extract sensitive data, inject backdoor or drop remote shells on android devices.

There may be some bugs on devices running Android 6.* Marshmallow because of new Android security policies. And *keypad* injection may not work depending on processing speed of device and version.

This program has been tested on Android 5.0.2 Lollipop on an LG H818n. It works flawlessly on many other devices but I do not guarantee this program will work for you flawlessly.

Do not hesitate in contacting me if you have any question o suggestion. E-Mail: jlrodriguezfragoso@gmail.com

[Here](http://jlrodriguezf.github.io/posts/WhatsPwn-2.0.html) is a tutorial for this tool.

###### This tool is currently in beta state, expect updates soon.

###### This tool is for educational and android developing purposes only. This was made to test and expose Android and WhatsApp security flaws.
	
---
# Changelog
##### WhatsPwn 2.0
###### Added
- New payload features: Inject meterpreter into legitimate apk, create hidden or visible payloads.
- Constant connection status.

###### Improved
- UI
- Fixed  a lot of major and minor bugs
- Only ask for connection when it needs it
- Autodetect local or wireless extraction

###### Removed
- Command line arguments

# Requirements
- Metasploit-Framework
- SSH & SSHPASS
- Ruby
- Apktool
	- Display manager running
- Java 7

# License and Disclaimer
See the [license](https://github.com/jlrodriguezf/WhatsPwn/blob/master/LICENSE) file or the [disclaimer](https://github.com/jlrodriguezf/WhatsPwn/blob/master/bin/DISCLAIMER) file.
