# Windows 10X on X86-64
<img src="https://github.com/daviiid99/Windows-10X-Builds/blob/main/Sources/logo.png">

amd64 (X86-64) images based on Windows 10X Insider Previews.
<br/>
<br/>

# Instructions

0.- Download the latest Windows 10X build from <a href="https://github.com/daviiid99/Windows-10X-Builds/releases/tag/20279">Releases</a><br/>

1.- On a Windows 10 PC, download and install the <a href="https://github.com/daviiid99/Windows-10X-Builds/blob/main/Tools/adksetup.exe">ADK</a> and <a href="https://github.com/daviiid99/Windows-10X-Builds/blob/main/Tools/adkwinpesetup.exe">WinPE</a> tools (needed to create the USB installer)<br/>

2.- Go to Start and search "Deployment and Imaging Tools Environment". Open it.

3.- In that window, type the following replacing the letter "X" with your Windows 10 drive disk<br/>

```cd c:\Program Files (x86)\Windows Kits\Tools\bin\i386```<br/>
```"%ProgramFiles(x86)%\Windows Kits\10\Assessment and Deployment Kit\Deployment Tools\%PROCESSOR_ARCHITECTURE%\DISM\wimmountadksetup%PROCESSOR_ARCHITECTURE%.exe" /q /uninstall```<br/>

4.- In the same window, now type ```diskpart```<br/>

5.- Attach the USB drive where you want create the installer and type the following lines:<br/>
```
#Steps to create the partitions
list disk (to find your USB drive letter)
select disk X (X is the USB drive that you attached before)
clean
create partition primary size=2048
active
format fs=FAT32 quick label="WinPE"
assign letter=P
create partition primary
format fs=NTFS quick label="Images"
assign letter=I
exit

#Steps to set up` WinPE/installations files
copy amd64 C:\WinPE_amd64
MakeWinPEMedia /UFD C:\WinPE_amd64 P:

```
<br/>

6.- Now, copy the Generic.ffu image you downloaded above to the I: partition of your USB drive.

7.- Shutdown your Laptop and enter your BIOS (Access key is OEM dependent:
```
ASRock: F2 or DEL
ASUS: F2 for all PCs, F2 or DEL for Motherboards
Acer: F2 or DEL
Dell: F2 or F12
ECS: DEL
Gigabyte / Aorus: F2 or DEL
HP: F10
Lenovo (Consumer Laptops): F2 or Fn + F2
Lenovo (Desktops): F1
Lenovo (ThinkPads): Enter then F1.
MSI: DEL for motherboards and PCs
Microsoft Surface Tablets: Press and hold volume up button.
Origin PC: F2
Samsung: F2
Toshiba: F2
Zotac: DEL
```

8.- In your BIOS, disable the ```WINDOWS VIRTUAL TECHNOLOGY``` for now (you may or may not need this option disabled to reach the installer)and enable the booting menu key.
<br/>

9.- Boot your USB drive and WinPE will boot on your laptop if you followed the steps above correctly.
<br/>

10.- In this window, type ```diskpart```
In diskpart, type:<br/>
```
list volume #take note of your main drive number
list drive #take note of your NTFS partitions letter
exit
```

11.- To start the installation process type the following, replacing the X (with your NTFS partition) and the Z (with your drive number):<br/>
```
dism /apply-ffu /imagefile:X:\\Generic.ffu /applydrive:\\.\physicaldriveZ
```
<br/>

12.- Once complete, remove your USB drive, type ```exit``` and cross your fingers.
<br/>

13.- Windows 10X will boot after some time, if you don't reach the setup on the first boot, please just wait 15-20 minutes on the black screen before rebooting or your Windows Boot Manager will enter on corruption state and you may need to repeat the flash process from step 9.
