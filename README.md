## Minisforum MS-A2 Windows 2025 Driver Installation
- The machine comes preloaded with Windows 11. I want Windows Server 2025 24H2 and blindly erased everything on the disk's pre installed OS. I was supporting someone over a voice only phone call doing the physical installation. 
- The machine is being built to replace an very old HPE Gen 8 computer running (8) Hyper-V VDI desktops in a domain environment with DUO 2 factor authentication. 
- Although I did install the Wi-Fi and Bluetooth drivers, I will be removing the physical mini PCIe card from the chassis, followed by the hardware device removal using Device Manager. 
- Please ensure 7-Zip is installed on the computer as it will be used later. (administrator cmd prompt: winget install 7zip.7zip)
- Ensure the MS-A2 drivers are downloaded
(https://pc-file.s3.us-west-1.amazonaws.com/MS-A2/Driver/F1WSA_AMD_MINISFORUM_Drivers_x64_250416.zip)
- Extract the files to a local directory on the machine. I've chosen C:\Installs
- Hack as demonstrated by Youtube's @einsteinagogo,
    Admin cmd prompt to disable driver signing:

    bcdedit -set loadoptions DISABLE_INTEGRITY_CHECKS

    bcdedit -set TESTSIGNING ON

    (No reboot is required)

    Then Install the Intel I225-LM manually: Computer Management/Device Manager/Other Devices: Update Devices/Browse my computer/Let me pick from a list/Network adapters/Have disk pointing to:
    
    C:\Installs\F1WSA_AMD_MINISFORUM_Drivers_x64_250416\Intel_Ethernet\Drivers\PRO2500\Winx64\W11
    
    Click OK. Then scroll the list for I225-LM. Click Next.
    Then answer Yes on the warning message.

    Restore driver signing from an Admin cmd prompt:

    bcdedit -set loadoptions ENABLE_INTEGRITY_CHECKS

    bcdedit -set TESTSIGNING OFF

    (Again no reboot is required)

- The supplied Chipset package failes to install. Use 7-Zip to extract the package installation executable from C:\Installs\F1WSA_AMD_MINISFORUM_Drivers_x64_250416\AMD_Chipset. Then navigate to:

    C:\Installs\F1WSA_AMD_MINISFORUM_Drivers_x64_250416\AMD_Chipset\AMD_Chipset_Software 
    
    Next execute: AMD_Chipset_Drivers.exe and follow the prompts. It will create a directory at C:\AMD containing the chipset drivers. I typicall reboot the computer after chipset drivers installation but it can be deferred for now.
- The Other Devices needs to update the AMD PSP 11.0 Device. Try updating the device pointing to C:\AMD and scanning folders. Alternatively, download it from Windows Catalog: 

    https://www.catalog.update.microsoft.com/Search.aspx?q=VEN_1022%26DEV_1649

    Extract the CAB file with 7-Zip. Then use that source location to update the PSP device driver.
- The next failed installation is the supplied display driver. Download from AMD:

    https://www.amd.com/en/resources/support-articles/release-notes/RN-PRO-WIN-25-Q3.html (released 2025-08-21)

    Selecting: **Microsoft Windows Server 2025 64-bit**.
    Then Install the download as usual.
- The audio drive can further be installed:
    C:\Installs\F1WSA_AMD_MINISFORUM_Drivers_x64_250416\Sensary_Audio\Drivers\Install.bat
- The remaining Other device has the path of:

    ACPI\VEN_MSFT&DEV_0200

    Researching the id information points to the Microsoft Pluton security device that is "only a Windows 11" feature. I believe it is safe to ignore the unknown device under Windows Server 2025. TPM 2.0 and AMD PSP both show installed under: Security devices
