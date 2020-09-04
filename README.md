##Problems that may meet in process of installing ubuntu alongside Win10?
Steps for Windows10 can refer to : https://www.zhihu.com/question/54059979
Steps for Ubuntu can refer to: https://www.jianshu.com/p/54d9a3a695cc
1. Why there is no EFI partition when we divided the disk?
Solution:
  (1) The EFI format of USB is necessary when we make a USB. The Rufus version after 3.11 will ask user to choose GPT or MBR type they would like. Please choose GPT, which is for UEFI boot in a computer. Users who omit choosing GPT will get a MBR partition and finally will no longer meet EFI system partition during installing. Previous version before 3.0 has default partition type, so notice should be given in this part.
 
 
   (2) Just as another user mentioned, several rules should be followed:
1) The computer you make the boot disk needs to boot from uefi
2) Use uefi to make a boot disk
3) Use uefi to load the boot disk (bios setting, I am Dell, press F12 to dynamically select the way to boot the U disk)
4) You can see the uefi partition during installation, choose to install the launcher to this partition.
https://blog.csdn.net/by674868212/article/details/89855791?utm_medium=distribute.pc_relevant_bbs_down.none-task-blog-baidujs-2.nonecase&depth_1-utm_source=distribute.pc_relevant_bbs_down.none-task-blog-baidujs-2.nonecase

2. Which boot way should we choose “EFI:USB” or USB without EFI?
Solution: In Q1, we made USB disk in EFI format. SO here EFI:USB should be chosen.
3. Why computer can not boot normally after changing BIOS menu?
Solution: There are relationships between partition disk type and boot type. For example, if EFI was chosen to with GPT format partition disk type, but legacy (which means old version) should be cooperate with MBR format. If the correspondence is not correction, the computer will not boot both windows and Ubuntu.

4. BIOS setting: F2. If we break the windows boot program, we can add boot item in this window.
