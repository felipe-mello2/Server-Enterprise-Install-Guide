# Server-Enterprise-Install-Guide
**ABOUT**
- LTSC (Long-Term Servicing Channel) and Windows Server editions are streamlined versions of Windows that focus on stability and long-term support.
They receive no feature updates, only security updates, and are supported for up to 10 years.
These editions are designed to minimize bloat, making them ideal for enterprise environments and servers where stability and security are prioritized over new features.
Assuming you don't need certain Windows features, these editions are excellent for gamers who want a minimal, bloat-free, and stable operating system.

**DOWNLOADS**
- **W10 Server 21h2 2022** https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022 <br>
Click "Download the ISO" <br>
Enter fake information <br>
Click "ISO download, 64-bit edition"

- **W11 Server 24h2 2025** https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025 <br>
Click "Download the ISO" <br>
Enter fake information <br>
Click "ISO download, 64-bit edition"

- **W10 LTSC 21h2 2021** https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise <br>
Click "Download the ISO – LTSC Enterprise" <br>
Enter fake information <br>
Click "ISO – Enterprise LTSC downloads, 64-bit edition"

- **W11 LTSC 23h2 2024** https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-iot-enterprise-ltsc <br>
Click "Download Windows 11 IoT Enterprise LTSC" <br>
Enter fake information <br>
Click "ISO - Windows 11 IoT Enterprise LTSC 2024 Evaluation, x64 or AMD64 edition"

**BURN TO USB**
- **Rufus** https://rufus.ie/en/ <br>
Click and download "Portable, Windows x64" <br>
Plug in USB <br>
Open Rufus <br>
Select USB <br>
Select ISO <br>
Similar to this <br>
![image](https://github.com/user-attachments/assets/1c37c78f-395f-46a7-b04d-c2c0448d9336) <br>
Click start <br>
Leave these "autounattended" settings blank or you will brick the install <br>
![image](https://github.com/user-attachments/assets/f4777d16-7370-426e-902c-a5792b35e9c1) <br>
Disable bitlocker in windows settings (if on) <br>
![image](https://github.com/user-attachments/assets/a32a984d-7cce-4ba8-a5fa-5bbbbdb005ad) <br>
Backup any files to USB (if needed)

**PREPARE ETHERNET DRIVERS**
- Some ethernet cards wont work on server such as "i225-V, i226-V" <br>
Here is a work around <br> 
https://www.thomas-krenn.com/de/wiki/Intel_i225-V_und_i226-V_Treiber_in_Windows_Server_2022_installieren <br>
https://www.reddit.com/r/WindowsServer2019/comments/tvyquw/here_is_an_intel_i225v_mod_driver_for_windows <br>
Download latest driver for your ethernet card, extract it <br>
Edit "e2f.inf" in notepad <br>
Under **"[Intel.NTamd64.10.0...17763]"** add . . .

- **%E15F3NC.DeviceDesc% = E15F3.10.0.1..17763, PCI\VEN_8086&DEV_15F3&REV_01 <br>
%E15F3_2NC.DeviceDesc% = E15F3_2.10.0.1..17763, PCI\VEN_8086&DEV_15F3&REV_02 <br>
%E15F3_3NC.DeviceDesc% = E15F3_3.10.0.1..17763, PCI\VEN_8086&DEV_15F3&REV_03 <br>
%E125CNC.DeviceDesc% = E125C.10.0.1..17763, PCI\VEN_8086&DEV_125C**

- A simmilar method for other ethernet cards can be used if you find the device manager ID'S <br>
Save file, drag driver files to the USB Stick <br>
As these drivers are unsigned, they will need to be installed with driver signature off

- **OFF**
In cmd or powershell <br>
bcdedit -set loadoptions DISABLE_INTEGRITY_CHECKS <br>
bcdedit -set TESTSIGNING ON <br>
bcdedit -set NOINTEGRITYCHECKS ON <br>
restart <br>
install driver via device manager

- **ON** In cmd or powershell <br>
bcdedit /deletevalue loadoptions <br>
bcdedit -set TESTSIGNING OFF <br>
bcdedit -set NOINTEGRITYCHECKS OFF <br>
restart

**TO BIOS**
- Restart PC, spam F2 or DEL and go to bios <br>
Enable TPM (for W11 builds) <br>
Disable secure boot (for some oem prebuilts & laptops) <br>
Change boot order to USB <br>
Install the ISO (Server users, make sure to install desktop version)

**CONVERT EVAL TO NORMAL EDITION** <br>
- https://massgrave.dev/kms38 <br>
In powershell run this <br>
**Install-WindowsFeature -Name VolumeActivation -IncludeAllSubFeature –IncludeManagementTools**

- In powershell run this <br>
W10 Server 21h2 2022 <br>
**dism /online /set-edition:ServerStandard /productkey:VDYBN-27WPP-V4HQT-9VMD4-VMK7H /accepteula** <br>
W11 Server 24h2 2025 <br>
**dism /online /set-edition:ServerStandard /productkey:TVRH6-WHNXV-R9WG3-9XRFY-MY832 /accepteula** <br>
W10 LTSC 21h2 2021 <br>
**dism /online /set-edition:ServerStandard /productkey:KBN8V-HFGQ4-MGXVD-347P6-PDQGT /accepteula** <br>
W11 LTSC 23h2 2024 <br>
**dism /online /set-edition:ServerStandard /productkey:KBN8V-HFGQ4-MGXVD-347P6-PDQGT /accepteula** <br>
Restart

**ACTIVATE**
- https://github.com/massgravel/Microsoft-Activation-Scripts
- In powershell run this
- **irm https://get.activated.win | iex**
- Select 3, then 1

**GEFORCE NOT WORKING ON W10 Server 21h2 2022**
- In powershell run this
- **Install-WindowsFeature -Name Wireless-Networking**

**AMD/INTEL CHIPSET DRIVERS NOT INSTALLING/WORKING**
- Extract files
On AMD these may be already extracted if you ran the installer
Found in "C:\AMD\Chipset_Software\Packages\IODriver"
Install from device manager
 
**AMD GRAPHICS DRIVERS NOT INSTALLING/WORKING**
- Extract files
Install from device manager

**OTHER DRIVERS**
- Snappy Driver Installer Origin <br>
https://www.glenn.delahoy.com/snappy-driver-installer-origin/

**REMOVE SERVER MANAGER ON BOOT**
![image](https://github.com/user-attachments/assets/3500f7f6-0ced-4524-b8ef-316d167da885)

**REMOVE PASSWORD REQUIREMENT**
- Run "gpedit.msc" <br>
![image](https://github.com/user-attachments/assets/ec992915-c0a7-498f-800f-e76164d6a208)

- Change new password to blank
![image](https://github.com/user-attachments/assets/d2e98128-369d-4f74-abd3-6bdf40dc058c)

**DISABLE SHUTDOWN EVENT TRACKER**
- Run "gpedit.msc"
![image](https://github.com/user-attachments/assets/c33d3828-a006-4178-ad1e-13a626489c2d)
