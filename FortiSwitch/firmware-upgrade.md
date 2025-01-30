# Firmware Upgrade

There is a recommended order how for upgrading a managed FortiSwitch environment. However, ignoring this might have no impact on smaller firmware steps, but it is highly recommended for larger setups and significant firmware uprades, especially between major Firmware releases, such as 6.4.3 to 7.2.9

Upgrade Fortigate first and then work your way out from the access switches back to the core switches. It is highly recommended to upgrade and restart switches in an MCLAG configuration simultaneously. MCLAG configurations with switches running different firmware versions are not supported. I recommend using firmware staging for MCLAG setups.

Fortinet maintains a list of recommended Fortigate and FortiSwitch version combinations. Just search for 'FortiLink Compatibility' on the internet; it should provide the latest compatibility chart.

Upgrading firmware generally does not require any intermediate steps. Fortiswitches can be upgraded from version 3.5.0 directly to the latest release. The only current exceptions are the FS-424E (incl. POE, FPOE, Fiber) and FS-M426-FPOE models. Be sure to check the release notes.

## Firmware Staging

List currently installed Fortiswitch images on the Fortigate:

```
FW01 (global) # execute switch-controller switch-software list-available 

ImageName                              ImageSize(B)   ImageInfo               Uploaded Time  
S448EF-v7.2-build495-IMG.swtp          35920005       S448EF-v7.2-build495    Mon Aug 19 15:37:26 2024
S148FN-v7.2-build495-IMG.swtp          19860661       S148FN-v7.2-build495    Mon Aug 19 15:37:52 2024
```

Upload new images to the FortiGate. It is recommended to use FTP for faster transfer. [BabyFTP ](https://www.pablosoftwaresolutions.com/html/baby_ftp_server.html)is suggested for setting up a one-time use FTP server.

```
FW01 (global) # execute switch-controller switch-software upload ftp FSW_1024E-v7-build0495-FORTINET.out 10.1.20.61
Please wait...

Connect to ftp server 10.1.20.61 ...
Get wtp image file from ftp server OK.
Image checking ...
Image MD5 calculating ...
Image Saving FS1E24-v7.2-build495-IMG.swtp ...
Successful!
```

Stage the firmware to all FortiSwitches. This will store the image on the FortiSwitch in the backup partition and set it as the default boot partition for the next switch restart.\
Rerun the `execute switch-controller switch-software list-available` command to get the new image name.

```
FW01 (root) # execute switch-controller switch-software stage all S448EF-v7.2-build495-IMG.swtp
Staged Image Version S448EF-v7.2-build495
Image staging operation is started for FortiSwitch S448EFTF20001231 ...
Image staging operation is started for FortiSwitch S448EFTF20001232 ...
Image staging operation is started for FortiSwitch S448EFTF20001233 ...
Image staging operation is started for FortiSwitch S448EFTF20001234 ...
```

Verify the images are staged correctly

```
FW01 (root) # execute switch-controller get-upgrade-status 
                Device    Running-version                                Status      Next-boot
===================================================================================================================
VDOM : root
        FS1E48T100000035  FS1E48-v7.2.5-build453,230707 (GA)             (0/100/100)   FS1E48-v7.2-build495     (Idle) 
        FS1E48T100000036  FS1E48-v7.2.5-build453,230707 (GA)             (0/100/100)   FS1E48-v7.2-build495     (Idle) 
        S448EFTF10000040  S448EF-v7.2.5-build453,230707 (GA)             (0/100/100)   S448EF-v7.2-build495     (Idle) 
        S448EFTF10000067  S448EF-v7.2.5-build453,230707 (GA)             (0/100/100)   S448EF-v7.2-build495     (Idle) 
        S448EFTF10000018  S448EF-v7.2.5-build453,230707 (GA)             (0/100/100)   S448EF-v7.2-build495     (Idle) 
        S448EFTF10000021  S448EF-v7.2.5-build453,230707 (GA)             (0/100/100)   S448EF-v7.2-build495     (Idle) 
        S448EFTF10000065  S448EF-v7.2.5-build453,230707 (GA)             (0/100/100)   S448EF-v7.2-build495     (Idle) 
```

Reboot the switches from the GUI, starting with the access switches and working your way back to the core. In a multi-tier setup, reboot the access switches first, then the second-tier core switch, and finally the first-tier core switch. Reboot MCLAG switches at the same time.\


In the List View under 'Managed FortiSwitches' you can select multiple switches and reboot them at the same time.

\
Alternatively, the switches can be restarted from the Fortigate CLI:

`execute switch-controller switch-action restart swtp all`\
`execute switch-controller switch-action restart swtp switch-group <switchgroups>`
