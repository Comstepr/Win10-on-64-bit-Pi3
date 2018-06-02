**Windows 10 on Raspberry Pi 3**

**Note: I am not responsible for anything that happens to your Raspberry Pi. The steps listed have not been tested and may not work.**
**It is recommended to follow Davindra's tutorial. https://www.youtube.com/watch?v=c0VqVm8X_zQ Note: I am not Davindra**


**Prerequisties**

- Some time (a few hours)
- An SD card (16GB is fine, but you can use an SD card that is bigger than 16GB)

**Steps:**

1. Download MiniTool Partition Wizard https://www.partitionwizard.com/free-partition-manager.html
2. Download this repository and extract the files into a folder
3. Extract "install.wim - Extract.7z"
4. Open MiniTool Partition Wizard.
5. Plug in your SD card into your computer.
6. Locate your SD card in the Partition Wizard and delete all partitions. **Note: This will remove all data.**
7. Create a partition that is more than 100MB and name it BOOT.
8. Select Primary and set the file system as FAT32.
9. Click OK.
10. Create a partition with the remaining storage on the SD card and name it Windows.
11. Select Primary and set the file system as NTFS.
12. Click OK.
13. Click Apply.
14. Copy the UEFI files to the drive of the BOOT partition on the SD card.
15. Open a command prompt as administrator.
16. In the command prompt, navigate to the folder you extracted the "install.wim - Extract.7z" file to.
17. Run the command 
```
dism /apply-image /imagefile:install.wim /index:1 /applydir:<**the drive letter of the Windows partition on the SD card**>:\
```
18. Run the command 
```
bcdboot <**the drive letter of the Windows partition on the SD card**>:\Windows /s <**the drive letter of the BOOT partition on the SD card**>: /f UEFI
```
19. Run the command 
```
bcdedit /store <**the drive letter of the BOOT partition on the SD card**>:\EFI\Microsoft\Boot\bcd /set {default} testsigning on
```
20. Run the command 
```
bcdedit /store <**the drive letter of the BOOT partition on the SD card**>:\EFI\Microsoft\Boot\bcd /set {default} nointegritychecks on
```
21. Copy the finish.reg to the **drive of the Windows partition on the SD card.**
22. Eject your SD card and insert it into your Raspberry Pi 3.
23. Turn on your Raspberry Pi 3. It may take a couple of minutes (or longer) to boot.
24. When you see the error "Windows installation cannot proceed", press SHIFT+F10. The command prompt will open.
25. In the command prompt, enter 
```
mmc
```
26. In mmc, click File, Add/Remove Snap-ins.
27. Double-click Computer Management.
28. Click Finish.
29. Click Ok on the dialog box that says Event Viewer.
30. Click Ok on the Add/Remove Snap-ins menu.
31. In mmc, click Computer Management (Local), Local Users and Groups, Users.
32. Double-click on the Administrator account.
33. Uncheck Account is disabled.
34. Right-click on the Administrator account and click Set Password.
35. Click Proceed.
36. Enter your password.
37. Click Ok on the Set Password dialog box.
38. Click Ok on the Local Users and Groups dialog box.
39. Close mmc.
40. Click No.
41. In the command prompt, navigate to the folder you copied the finish.reg to.
42. In the command prompt, enter 
```
regedit finish.reg.
```
43. Restart the Raspberry Pi or enter 
```
shutdown /r /t 0
```
in the command prompt to restart it.






Credits to:

https://github.com/andreiw/RaspberryPiPkg
https://github.com/andreiw/rpi3winstuff
https://uup.rg-adguard.net/
https://sourceforge.net/p/windows-10-lite/wiki/Finish%20setup%20immediately/
https://www.youtube.com/watch?v=c0VqVm8X_zQ
