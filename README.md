# ASUS-PRIME-Z390-A-HACKINTOSH-Clover-iGPU-with-dGPU-UHD630-Radeon VII


## Components
- M/B: ASUS PRIME Z390-A(BIOS Ver. 1005)
- CPU: Intel® Core™ i9-9900K Processor
- iGPU: Intel® UHD Graphics 630
- dGPU: AMD Radeon VII 16GB (Public Edition)
- Lan: Intel® I219V, 1 x Gigabit LAN Controller
- WiFi/Bluetooth: BCM94360CS2
- Audio: Realtek® ALC S1220A 8-Channel High Definition Audio
- Case: Fractal Design R5 Blackout


## Using
Copy directory ```BOOT``` and ```CLOVER``` to your EFI.

## Comments
This Hackintosh build guide is NOT GUARANTEE 100% fully working in your conditions.

This guide has been tested on MacOS Mojave 10.14.6 and 10.15 bata 8 and prefers the use of an AMD dGPU for ease of installation. This settings are considered using dGPU(RX580) and iGPU(UHD 630 with Quicksync) simultaneously in specifically the FCPX(10.14.6) and these dual GPUs can nearly full hardware accelerated.(similar to the Headless setting but not the exact same)
And this guide can be used on the Gigabyte M/B also. (some settings different)

Special Thanks to CaseySJ(at tonymacx86.com) and Newlife(at x86.co.kr)

**I forked the project from [ASUS-PRIME-Z390-A-HACKINTOSH-Clover-iGPU-with-dGPU-UHD630-RX580](https://github.com/dhckdgjs/ASUS-PRIME-Z390-A-HACKINTOSH-Clover-iGPU-with-dGPU-UHD630-RX580), it work on my computer, and I add support to 10.15.3.**


## Procedure
1. Install MacOS 10.15.3(with Lilu.kext and Whatevergreen.kext )

// No need to do this. There is no solution to remove WEG with Radeon VII currently.

2. Set the ACPI settings as below
```
				<dict>
					<key>Comment</key>
					<string>change HECI to IMEI</string>
					<key>Disabled</key>
					<false/>
					<key>Find</key>
					<data>
					SEVDSQ==
					</data>
					<key>Replace</key>
					<data>
					SU1FSQ==
					</data>
				</dict>
				<dict>
					<key>Comment</key>
					<string>change MEI to IMEI</string>
					<key>Disabled</key>
					<false/>
					<key>Find</key>
					<data>
					TUVJXw==
					</data>
					<key>Replace</key>
					<data>
					SU1FSQ==
					</data>
				</dict>
				<dict>
					<key>Comment</key>
					<string>change GFX0 to IGPU</string>
					<key>Disabled</key>
					<false/>
					<key>Find</key>
					<data>
					R0ZYMA==
					</data>
					<key>Replace</key>
					<data>
					SUdQVQ==
					</data>
				</dict>
				<dict>
					<key>Comment</key>
					<string>change PEGP to GFX0</string>
					<key>Disabled</key>
					<false/>
					<key>Find</key>
					<data>
					UEVHUA==
					</data>
					<key>Replace</key>
					<data>
					R0ZYMA==
					</data>
				</dict>
```


3. Add Kext patch to the Kernel and Kext Patches
```
			<dict>
				<key>Comment</key>
				<string>Black Screen Patch Vega 56/64, RX580 etc. (c)Pike R. Alpha</string>
				<key>Disabled</key>
				<false/>
				<key>Find</key>
				<data>
				Ym9hcmQtaWQ=
				</data>
				<key>InfoPlistPatch</key>
				<false/>
				<key>Name</key>
				<string>AppleGraphicsDevicePolicy</string>
				<key>Replace</key>
				<data>
				Ym9hcmQtaXg=
				</data>
			</dict>
```


4. Set the SMBIOS to iMac 19,1
5. Change M/B Bios Primary display(ASUS) or Initial Display Output(Gigabyte) to PEG or PCIE(iGPU MUST be turn on)
6. Apply the Framebuffer patch(Headless) on your Devices setting  in config.plist. (check settings as below and recommend using the Hackintool) 
```
platform-id: 0x3E910003(maybe works well 0x3E920003)

device-id: 0x3E91 Intel UHD 630(maybe works well 0x3E92)

Check some options:
	- General: DeviceProperties, Connectors, VRAM, Graphic Device
	- Advanced: Hotplug Reboot Fix(I’m not sure), Spoof Video Device ID, VRAM 2048 MB, GfxYTile Fix
```

Ref. [GUIDE General Framebuffer Patching Guide (HDMI Black Screen Problem) | tonymacx86.com](https://www.tonymacx86.com/threads/guide-general-framebuffer-patching-guide-hdmi-black-screen-problem.269149/)

7. Remove Whatevergreen.kext on your EFI/CLOVER/kexts/Other
8. Done


## Summary
As far as I have been able to test, everything works well except Thunderbolt devices.(I don’t have ThunderboltEX 3 or Titan/Alpine ridge add-on card)
If you need other detailed settings fot this ASUS PRIME Z390-A M/B, please check released file.(like as USBport)
Thanks.



## Screenshots

10.15
![Sys_info](./pics/Sys_info.png)
![Sys_info_PCI](./pics/Sys_info_PCI.png)
![Sys_info_USB](./pics/Sys_info_USB.png)
![Sys_info_NVME](./pics/Sys_info_NVME.png)

Not perfect (has some bug with HDMI audio and some animation failure) graphic information.

![Sys_info_graphics](./pics/Sys_info_graphics.png)

Perfect grafic information.

![Sys_info_graphics_ok](./pics/Sys_info_graphic_ok.png)

10.14
![Sys info_PCI](https://user-images.githubusercontent.com/35429874/61994177-59df8980-b0b2-11e9-857f-47d757fa7a0f.png)
![Sys info_USB](https://user-images.githubusercontent.com/35429874/61994187-6c59c300-b0b2-11e9-896a-8a3ac4609117.png)
![Sys info_Audio](https://user-images.githubusercontent.com/35429874/61994188-711e7700-b0b2-11e9-908f-1ffd44d945a8.png)
![Sys info_Bluetooth](https://user-images.githubusercontent.com/35429874/61994190-71b70d80-b0b2-11e9-8f2a-18757d28cd83.png)
![Sys info_Network](https://user-images.githubusercontent.com/35429874/61994191-71b70d80-b0b2-11e9-888d-b25cac1842a8.png)
![PRIME Z390-A - USB port map final](https://user-images.githubusercontent.com/35429874/61994465-821cb780-b0b5-11e9-9d00-b12ed9046afc.jpg)
