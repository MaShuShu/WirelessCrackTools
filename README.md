WirelessCrackCommandlineTools for Android(arm+bcm4329/4330)

此目录下的zip文件为无线破解命令行工具卡刷包，以md5为后缀名的文件为这些文件的md5值比对表

安装:通过第三方recovery卡刷安装WirelessCrackTools-Installer.zip

卸载:通过第三方recovery卡刷安装WirelessCrackTools-Uninstaller.zip

该工具包中(WirelessCrackTools-Installer.zip)包含aircrack-ng,reaver,wash,pixiewps,iw等31个用于无线破解的命令行工具
安装成功后，再重启回到系统自行下载并安装termux(5.0+)之类的终端软件，至此可以开始使用这些命令行工具了

注意：若手机的网卡芯片不是bcm4329或bcm4330型号的，那么抓包的操作将不被支持，但跑包操作可以用aircrack-ng这个命令
安装该工具包的手机务必拥有root权限，安卓系统版本务必高于4.0，尽可能选用网卡芯片符合以上要求的手机，如:魅族2，Nexus 7等！

该工具包中的可执行程序是由kriswebdev提供的源码编译而来

## Aircrack-ng
> Aircrack-ng is an 802.11 WEP and WPA-PSK keys cracking program that can recover keys once enough data packets have been captured. It implements the standard FMS attack along with some optimizations like KoreK attacks, as well as the PTW attack, thus making the attack much faster compared to other WEP cracking tools.

# Running Aircrack-ng on Android (precompiled)

## Pre-requisites

 1. Device with **WiFi chipset, firmware & driver that support monitor-mode**
   * As of early 2016, only mass-market compatible devices are thoses having dedicated **Broadcom 4329** or **Broadcom 4330** WiFi chipsets (**Samsung Galaxy S1, Samsung Galaxy S2, Nexus 7, Huawei Honor**). Bcmon team has developed firmware and driver hacks for these chipsets. More recent devices have WiFi digital signal processed by the ARM CPU (Qualcomm or Samsung) and there is no publicily knowed monitor mode hacks for these devices at this time.
   * Otherwise, go look for **USB WiFi adapter** known to provide WiFi monitor-mode and injection support on Android.
 2. **Wireless extensions** enabled in Android kernel
  * That's normally bundled with the loaders/kernels below.
 3. Monitor-mode **firmware & driver loader**
   * Broadcom 4329: Bcmon won't load on CyanogenMod > v7 due to the move from bcm4329 driver to bcmdhd. For Galaxy S1, use [PwnAir](http://forum.xda-developers.com/showthread.php?t=2760170) on a KitKat ROM (or port the open source PwnAir kernel to more recent ROM following PwnAir kernel build instructions at the end of the XDA thread).
   * Broadcom 4330: Use [bcmon app](http://bcmon.blogspot.com/) (not maintained anymore by their owners) to load the monitor-mode firmware and driver.
   * USB WiFi adapter that supports monitor-mode: This completely depends on your USB WiFi adapter and is out of scope of this README. You will need to find and build the driver, along with your ROM kernel, from source. Otherwise you would probably face the *magic version* mismatch issue. As Android kernel is a Linux kernel afterall, you should succeed in compiling the driver from the Linux driver source. Either find a nice HowTo for compiling your USB WiFi adapter driver on a defined Android ROM, or struggle with the driver build guide of your Android ROM. Note that some guides use a chrooted Ubuntu or Kali as an alternative to building a pure Android driver.

## Install

* Load the monitor-mode firmware/driver
  * Automated: Install [bcmon app](http://bcmon.blogspot.com/) (bcm4330) or [PwnAir kernel+app](http://forum.xda-developers.com/showthread.php?t=2760170) (Samsung Galaxy S1 with KitKat ROM) and load the monitor-mode firmware/driver.

## Run

Check your wireless interface status (should be in "Mode: Monitor"):
```shell
su
iwconfig
```

Check Airodump is working:
    `airodump-ng eth0`

Provided your wireless interface is eth0.

If it is working, then check the Aircrack documentation for HowTo.

# Documentation

## Aircrack-ng official documentation 
> Documentation, tutorials, ... can be found on http://www.aircrack-ng.org
> See also manpages and the forum.


OVERVIEW

	Reaver performs a brute force attack against an access point's WiFi Protected Setup pin number. 
	Once the WPS pin is found, the WPA PSK can be recovered and alternately the AP's wireless settings can be 
	reconfigured.

	While Reaver does not support reconfiguring the AP, this can be accomplished with wpa_supplicant once 
	the WPS pin is known.

DESCRIPTION

	Reaver targets the external registrar functionality mandated by the WiFi Protected Setup specification.
	Access points will provide authenticated registrars with their current wireless configuration (including
	the WPA PSK), and also accept a new configuration from the registrar.

	In order to authenticate as a registrar, the registrar must prove its knowledge of the AP's 8-digit pin
	number. Registrars may authenticate themselves to an AP at any time without any user interaction. Because
	the WPS protocol is conducted over EAP, the registrar need only be associated with the AP and does not 
	need any prior knowledge of the wireless encryption or configuration.

	Reaver performs a brute force attack against the AP, attempting every possible combination in order to
	guess the AP's 8 digit pin number. Since the pin numbers are all numeric, there are 10^8 (100,000,000) 
	possible values for any given pin number. However, because the last digit of the pin is a checksum value
	which can be calculated based on the previous 7 digits, that key space is reduced to 10^7 (10,000,000) 
	possible values.

	The key space is reduced even further due to the fact that the WPS authentication protocol cuts the pin in
	half and validates each half individually. That means that there are 10^4 (10,000) possible values for the
	first half of the pin and 10^3 (1,000) possible values for the second half of the pin, with the last digit
	of the pin being a checksum.
	
	Reaver brute forces the first half of the pin and then the second half of the pin, meaning that the entire 
	key space for the WPS pin number can be exhausted in 11,000 attempts. The speed at which Reaver can test 
	pin numbers is entirely limited by the speed at which the AP can process WPS requests. Some APs are fast enough 
	that one pin can be tested every second; others are slower and only allow one pin every ten seconds. Statistically, 
	it will only take half of that time in order to guess the correct pin number.

KNOWN BUGS

	o Some drivers don't play nice with Reaver (check the wiki for the latest list)

	The lwe directory contains Wireless Tools version 29, used for interfacing with Linux Wireless Extensions.
REAVER USAGE

        Usually, the only required arguments to Reaver are the interface name and the BSSID of the target AP:

                # reaver -i mon0 -b 00:01:02:03:04:05

	It is suggested that you run Reaver in verbose mode in order to get more detailed information about
	the attack as it progresses:

		# reaver -i mon0 -b 00:01:02:03:04:05 -vv

        The channel and SSID (provided that the SSID is not cloaked) of the target AP will be automatically
        identified by Reaver, unless explicitly specified on the command line:

                # reaver -i mon0 -b 00:01:02:03:04:05 -c 11 -e linksys

        Since version 1.3, Reaver implements the small DH key optimization as suggested by Stefan which can
        speed up the attack speed:

                # reaver -i mon0 -b 00:01:02:03:04:05 --dh-small

        By default, if the AP switches channels, Reaver will also change its channel accordingly. However,
        this feature may be disabled by fixing the interface's channel:

                # reaver -i mon0 -b 00:01:02:03:04:05 --fixed

	When spoofing your MAC address, you must set the desired address to spoof using the ifconfig utility,
	and additionally tell Reaver what the spoofed address is:

		# reaver -i mon0 -b 00:01:02:03:04:05 --mac=AA:BB:CC:DD:EE:FF

	The default receive timeout period is 5 seconds. This timeout period can be set manually if necessary
        (minimum timeout period is 1 second):

                # reaver -i mon0 -b 00:01:02:03:04:05 -t 2

        The default delay period between pin attempts is 1 second. This value can be increased or decreased
        to any non-negative integer value. A value of zero means no delay:

                # reaver -i mon0 -b 00:01:02:03:04:05 -d 0

        Some APs will temporarily lock their WPS state, typically for five minutes or less, when "suspicious"
        activity is detected. By default when a locked state is detected, Reaver will check the state every
        315 seconds (5 minutes and 15 seconds) and not continue brute forcing pins until the WPS state is unlocked.
        This check can be increased or decreased to any non-negative integer value:

                # reaver -i mon0 -b 00:01:02:03:04:05 --lock-delay=250

        The default timeout period for receiving the M5 and M7 WPS response messages is .1 seconds. This
        timeout period can be set manually if necessary (max timeout period is 1 second):

                # reaver -i mon0 -b 00:01:02:03:04:05 -T .5

        Some poor WPS implementations will drop a connection on the floor when an invalid pin is supplied
        instead of responding with a NACK message as the specs dictate. To account for this, if an M5/M7 timeout
        is reached, it is treated the same as a NACK by default. However, if it is known that the target AP sends
        NACKS (most do), this feature can be disabled to ensure better reliability. This option is largely useless
        as Reaver will auto-detect if an AP properly responds with NACKs or not:

                # reaver -i mon0 -b 00:01:02:03:04:05 --nack

        While most APs don't care, sending an EAP FAIL message to close out a WPS session is sometimes necessary.
        By default this feature is disabled, but can be enabled for those APs that need it:

                # reaver -i mon0 -b 00:01:02:03:04:05 --eap-terminate

        When 10 consecutive unexpected WPS errors are encountered, a warning message will be displayed. Since this
        may be a sign that the AP is rate limiting pin attempts or simply being overloaded, a sleep can be put in
        place that will occur whenever these warning messages appear:

                # reaver -i mon0 -b 00:01:02:03:04:05 --fail-wait=360
WASH USAGE

	Wash is a utility for identifying WPS enabled access points. It can survey from a live interface:

		# wash -i mon0 

	Or it can scan a list of pcap files:

		# wash -f capture1.pcap capture2.pcap capture3.pcap

	Wash will only show access points that support WPS. Wash displays the following information for each
	discovered access point:

		BSSID		The BSSID of the AP
		Channel		The APs channel, as specified in the AP's beacon packet
		WPS Version	The WPS version supported by the AP
		WPS Locked	The locked status of WPS, as reported in the AP's beacon packet
		ESSID		The ESSID of the AP

	By default, wash will perform a passive survey. However, wash can be instructed to send probe requests
	to each AP in order to obtain more information about the AP:

		# wash -i mon0 --scan

	By sending probe requests, wash will illicit a probe response from each AP. For WPS-capable APs, the
	WPS information element typically contains additional information about the AP, including make, model,
	and version data. This data is stored in the survey table of the reaver.db database.

	The reaver.db SQLite database contains three tables:

		history		This table lists attack history, including percent complete and recovered WPA keys
		survey		This table is re-populated each time wash is run with detailed access point information
		status		This table is used to indicate the overall status of wash/reaver
