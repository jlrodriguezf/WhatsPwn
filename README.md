# Description
Linux tool used to extract sensitive data, inject backdoor or drop remote shells on android devices.

There may be some bugs on devices running Android 6.* Marshmallow because of new Android security policies. And KEYPAD injection may not work depending on processing speed of device and version.

This program has been tested on Android 5.0.2 Lollipop on an LG H818n. It works flawlessly on many other devices but I do not guarantee this program will work for you flawlessly.

Do not hesitate in contacting me if you have any question o suggestion. E-Mail: jlrodriguezfragoso@gmail.com

###### This tool is for educational and android developing purposes only. This was made to test and expose android security flaws.
	
---
	
# Usage
	./whatspwn [ACTIONS] [GENERAL ARGUMENTS] [ARGUMENTS]

##### ACTIONS

	* MAY USE WITH ARGUMENTS

	[1 | FULL] *                            >>>   Start full attack. {BACKDOOR > WHATSAPP > EXTRACTION}
	[2 | RELAUNCH]                          >>>   Relaunch backdoor apk (injected virus) or installs it 
	                                               if not found.
	[3 | SHELL]                             >>>   Spawn a device shell.
	[4 | RESTORE]                           >>>   Restore whatsapp apk in case of error or emergency.
	[5 | EXTRACTION] + [local | remote] *   >>>   Just extract sensitive data from device.
	[6 | UPLOAD]                            >>>   Upload database to server. (configured in server file)
	[i | -i | --interface | INTERFACE]      >>>   Initialize CLI interface.

##### GENERAL ARGUMENTS

	[-o | --output] + [PATH]                >>>   Specifies the path for extracted data.
	                                               (Must end with /) IF NOT SPECIFIED IT WILL USE 
	                                                DEFAULT IN "config" FILE.
	[-h | --help]                           >>>   Displays this message.

##### ARGUMENTS FOR FULL

	[-b | --use-backdoor | --backdoor]           >>>   Injects virus for future connections.
	[-e | --black-hole | --use-black-hole]       >>>   Extracts sensitive data from device.
	[-v | --victim] + [NAME OF THE VICTIM]       >>>   Specifies the name of the victim.
	[-d | --DCIM | --dcim] * ONLY IF EXTRACTION  >>>   Extracts DCIM photos.
	[-k | --backup] + [BACKUP FILENAME]          >>>   Specifies the filename for backup. IF NOT
	                                                    SPECIFIED IT WILL USE DEFAULT IN "config" FILE.
	[-u | --upload]                              >>>   Uploads the backup to server and 
	                                                    prompt for SSH password.
##### ARGUMENTS FOR EXTRACTION

	[-v | --victim] + [NAME OF THE VICTIM]       >>>   Specifies the name of the victim.
	[-d | --DCIM | --dcim] * ONLY IF EXTRACTION  >>>   Extracts DCIM photos.
	[-k | --backup] + [BACKUP FILENAME]          >>>   Specifies the filename for backup. IF NOT
	                                                    SPECIFIED IT WILL USE DEFAULT IN "config" FILE.
	[-u | --upload]                              >>>   Uploads the backup to server and 
	                                                    prompt for SSH password.

