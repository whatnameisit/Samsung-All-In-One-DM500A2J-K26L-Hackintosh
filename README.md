# Samsung All-In-One DM500A2J-K26L Hackintosh
 
 ## System specification
 
 1. Name:   Samsung All-In-One (AIO) DM500A2J-K26L
 2. CPU: Intel Pentium 3558U
 3. Graphics: Intel HD Graphics (Haswell)
 4. Ethernet: Realtek RTL8168 Gigabit Ethernet Controller
 5. Wi-Fi and Bluetooth: Qualcomm Atheros AR9565 / AR3012
 6. TV Tuner Card: Polaris AV Capture in mPCIe
 7. Audio: Realtek HD Audio (ALC282)
 
 ## Working
 
 1. Native power management with CPU spoofing
 2. Ethernet
 3. Wi-Fi by loading High Sierra's IO80211HostFamily.kext and modified AirportAtheros40.kext via OpenCore.
 4. Audio (Currently disabled with class-code of FFFFFFFF because there is noise when turning on and off)
 
 ## Not working
 
 1. The IGPU Intel HD Graphics (Haswell) has one report of working QE/CI which could be fake and for which the support is only coded for 10.12.6, 10.13.6, and 10.14-10.14.3. I will need to get a compatible GPU and connect it via mPCIe which was supplied with the TV tuner card.
 2. The support for the AR3012 bluetooth has been dropped since High Sierra. I will attach a bluetooth dongle on the EXP GDC USB slot.
 
 ## To-Do's
 
 1. Install a compatible dGPU
 2. Fix audio noise
 3. Make HfsPlus.efi function with OpenCore
 
 ## Credits
 
 Apple for macOS
 
 The Acidanthera team for OpenCore and kexts
 
 Other great contributors I have yet to mention
