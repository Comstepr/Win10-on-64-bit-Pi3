**Windows 10 on Raspberry Pi 3**

**Note: I am not responsible for anything that happens to your Raspberry Pi. The steps listed have not been tested and may not work.**
**You can follow Davindra's tutorial, however this tutorial is step-by-step and may be clearer. In Davindra's tutorial, the ISO was compiled, however you don't have to compile the ISO if you use this tutorial. https://www.youtube.com/watch?v=c0VqVm8X_zQ 
**Note: I am not Davindra**

**There are two methods, however it is recommended that you follow Method 1.**

**Method 1 (not easy, but recommended)**
**You may try method 2 if you want to. It is easier, but it is not recommended.**
**Don't download "install.wim - Extract.7z". If you want to use "install.wim - Extract.7z", follow Method 2.**

**Prerequisties**

- Some time (a few hours)
- An SD card (16GB is fine, but you can use an SD card that is bigger than 16GB)
- MiniTool Partition Wizard
- Win10-on-64-bit-Pi3 Zip file

**Steps:**


1. Download MiniTool Partition Wizard https://www.partitionwizard.com/free-partition-manager.html
2. Download this repository and extract the files into a folder. Rename the folder to WinRpi.
3. Open MiniTool Partition Wizard.
4. Plug in your SD card into your computer.
5. Locate your SD card in the Partition Wizard and delete all partitions. **Note: This will remove all data.**
6. Create a partition that is more than 100MB and name it BOOT.
7. Select Primary and set the file system as FAT32.
8. Click OK.
9. Create a partition with the remaining storage on the SD card and name it Windows.
10. Select Primary and set the file system as NTFS.
11. Click OK.
12. Click Apply.
13. Navigate to the folder where you extracted the files from this repository to and open up the UEFI files folder.
14. Copy all the files in the UEFI folder and paste it in the **BOOT drive of the SD card**.
15. Go to https://uup.rg-adguard.net/ and download Build 17134.1 [arm64] of Windows Insider build. Select your language and select **Windows 10 Professional** and choose **Download ISO compiler in OneClick! (unZIP -> RUN creatingISO.cmd)** and download the file.
16. Extract the ZIP file that you downloaded from the UUP.
17. Run creatingISO.cmd. The ISO will be created, however, it may take a long time.
18. In the WinRpi folder, create a folder called "m".
19. Right-click the ISO, Open With, Windows Explorer.
20. Copy the install.wim to the WinRpi folder.
21. Extract the Win10-on-64-bit-Pi3 Zip file.
22. Eject the ISO by right-clicking on it and choosing Eject.
23. Open a command prompt as administrator.
24. cd into the WinRpi folder.
25. Run the command
```
dism /mount-image /imagefile:install.wim /Index:1 /MountDir:m
```
26. Run the command
```
dism /image:m /add-driver /driver:system32 /recurse /forceunsigned
```
27. Run the command
```
dism /unmount-wim /mountdir:m /commit
```
28. Run the command
```
dism /apply-image /imagefile:install.wim /index:1 /applydir:<**the drive letter of the Windows partition on the SD card**>:\
```
29. Run the command
```
bcdboot <**the drive letter of the Windows partition on the SD card**>:\Windows /s <**the drive letter of the BOOT partition on the SD card**> /f UEFI
```
30. Run the command
```
bcdedit /store <**the drive letter of the BOOT partition on the SD card**>:\EFI\Microsoft\Boot\bcd /set {default} testsigning on
```
31. Run the command
```
bcdedit /store <**the drive letter of the BOOT partition on the SD card**>:\EFI\Microsoft\Boot\bcd /set {default} nointegritychecks on
```
32. In the WinRpi folder, copy the finish.reg file to the **main folder of the Windows partition on the SD card.**
33. Eject the SD card.
34. Insert the SD card into your Raspberry Pi.
35. Turn on your Raspberry Pi 3. It may take a couple of minutes (or longer) to boot.
36. When you see the error "Windows installation cannot proceed", press SHIFT+F10. The command prompt will open.
37. In the command prompt, enter 
```
mmc
```
38. In mmc, click File, Add/Remove Snap-ins.
39. Double-click Computer Management.
40. Click Finish.
41. Click Ok on the dialog box that says Event Viewer.
42. Click Ok on the Add/Remove Snap-ins menu.
43. In mmc, click Computer Management (Local), Local Users and Groups, Users.
44. Double-click on the Administrator account.
45. Uncheck Account is disabled.
46. Right-click on the Administrator account and click Set Password.
47. Click Proceed.
48. Enter your password. You may leave the password blank.
49. Click Ok on the Set Password dialog box.
50. Click Ok on the Local Users and Groups dialog box.
51. Close mmc.
52. Click No.
53. In the command prompt, navigate to the folder you copied the finish.reg to.
54. In the command prompt, enter 
```
regedit finish.reg
```
55. Restart the Raspberry Pi or enter 
```
shutdown /r /t 0
```
in the command prompt to restart it.
**Note: If entering the command does not restart Windows, restart it manually.**

**Method 2**
**Method 2 is faster than Method 1, but it is not recommended. Method 1 is recommended.**


**Prerequisties**

- Some time (a few hours)
- An SD card (16GB is fine, but you can use an SD card that is bigger than 16GB)
- MiniTool Partition Wizard
- "install.wim - Extract.7z"

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
36. Enter your password. You may leave the password blank.
37. Click Ok on the Set Password dialog box.
38. Click Ok on the Local Users and Groups dialog box.
39. Close mmc.
40. Click No.
41. In the command prompt, navigate to the folder you copied the finish.reg to.
42. In the command prompt, enter 
```
regedit finish.reg
```
43. Restart the Raspberry Pi or enter 
```
shutdown /r /t 0
```
in the command prompt to restart it.

**Errors:**
**Note: If you have any errors, please make a issue in this GitHub.**


| Issue         | Solution |
| ------------- | ------------- |
| Windows setup could not configure to run on this computer's hardware  | Compile the ISO and try again  |
| The specified domain either does not exist or could not be contacted  | Compile the ISO and try again  |
| BSOD - DRIVER_POWER_STATE_FAILURE  | Compile the ISO and try again  |




Credits to:

https://github.com/andreiw/RaspberryPiPkg
https://github.com/andreiw/rpi3winstuff
https://uup.rg-adguard.net/
https://sourceforge.net/p/windows-10-lite/wiki/Finish%20setup%20immediately/
https://www.youtube.com/watch?v=c0VqVm8X_zQ
