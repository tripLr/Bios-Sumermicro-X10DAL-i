# Bios-Sumermicro-X10DAL-i

Upgrade instructions and path for Supermicro bios problems 

PROBLEMS: 

If you have a Supermicro X10DAL-i ( or problems with other bios/boards ) 
    and the bios will not update 
    
    STOP and READ entire post, then do , DONT skip a step !!
    
 2016 bios has problems updating to the latest bios 
 Here is the specific workable steps in case 
   a. Bios update within bios does not work, freezes, or wont see the file
   b. Bios recovery does not work with the supermicro method of booting a SUPER.ROM recovery method
   c. Or you get into SUPER.ROM recovery and DO NOT SEE files. 
       If you DO get here, DO NOT MAKE CHANGES or save changes !
       YOU WILL CORRUPT your bios  !!! 
   d. you see corrupted output , or get a frozen output with a bootable DOS/FreeDos usb stick
       this is result of memory allocation for the Dos Boot drivers 
 
 STEP 1
 
 required :
     create a clean bootable Freedos USB stick with newest Freedos, Not the installer on rufus 
     1. download freedos full
     2. extract zip file for image
     3. write image to USB using Rufus ( do not create bootable rufus freedos contained in rufus )
                                           this wont work, tried it
     4. this creates a fat16 partition
     5. resize partition on USB stick if necessary
     6. change the USB stick from an installer to a usable bootable freedos USB thumb drive 
         
     Since the USB is an installer I found a method to convert it to a Boot disk
     Source:
       https://superuser.com/questions/1388931/how-to-install-freedos-onto-a-usb-stick  
       
[QUOTE]

    Unfortunately the current information on the FreeDOS Wiki is not up to date, but with the help from FreeDos Developer Jim Hall I could find the solution:
    
    Download the USB “Full” installer from the FreeDOS page.
    
    Unpack the downloaded zip
    
    Use a USB formatting tool (for example rufus) to write the image to USB (take care to write over the right drive)
    
    Move the directory D:\xxxxx\BIN to D:\BIN 
      :  > Depending on the release this BIN folder may be in a different location ◀️
    
    Create or Edit first and last two lines in D:\FDCONFIG.SYS as follows

[CODE]

    !COUNTRY=001,858:\BIN\COUNTRY.SYS
    !LASTDRIVE=Z
    !BUFFERS=20
    !FILES=40
    DOS=HIGH
    DOS=UMB
    DOSDATA=UMB
    DEVICE=\BIN\HIMEMX.EXE
    SHELLHIGH=COMMAND.COM \BIN /E:2048 /P=\AUTOEXEC.BAT

[/CODE]

Edit D:\AUTOEXEC.BAT as follows (Windows will hide this file, but you can open it by directly giving the filename). Only the line setting the DOSDIR needs to be changed and some display code at the end of AUTOEXEC.BAT are to be removed

[CODE]

    @echo off
    SET DOSDIR=
    SET LANG=
    SET PATH=%dosdir%\BIN
    SET DIRCMD=/P /OGN /Y
    rem SET TEMP=%dosdir%\TEMP
    rem SET TMP=%TEMP%
    rem SET NLSPATH=%dosdir%\NLS
    rem SET HELPPATH=%dosdir%\HELP
    rem SET BLASTER=A220 I5 D1 H5 P330
    rem SET COPYCMD=/-Y
    DEVLOAD /H /Q %dosdir%\BIN\UDVD2.SYS /D:FDCD0001
    SHSUCDX /QQ /D3
    rem SHSUCDHD /QQ /F:FDBOOTCD.ISO
    FDAPM APMDOS
    rem SHARE
    rem NLSFUNC %dosdir%\BIN\COUNTRY.SYS
    rem DISPLAY CON=(EGA),858,2)
    rem MODE CON CP PREP=((858) %dosdir%\CPI\EGA.CPX)
    rem KEYB US,858,%dosdir%\bin\keyboard.sys
    rem CHCP 858
    rem PCNTPK INT=0x60
    rem DHCP
    rem MOUSE
    rem DEVLOAD /H /Q %dosdir%\BIN\UIDE.SYS /H /D:FDCD0001 /S5
    SHSUCDX /QQ /~ /D:?SHSU-CDR,D /D:?SHSU-CDH,D /D:?FDCD0001,D /D:?FDCD0002,D /D:?FDCD0003,D
    rem MEM /C /N
    SHSUCDX /D
    rem DOSLFN
    rem LBACACHE.COM buf 20 flop
    SET AUTOFILE=%0
    SET CFGFILE=\FDCONFIG.SYS
    alias reboot=fdapm warmboot
    alias reset=fdisk /reboot
    alias halt=fdapm poweroff
    alias shutdown=fdapm poweroff
    rem alias cfg=edit %cfgfile%
    rem alias auto=edit %0
    vecho /p Done processing startup files /fCyan FDCONFIG.SYS /a7 and /fCyan AUTOEXEC.BAT /a7/p

[/CODE]

Delete D:\SETUP.BAT

Done, safe your files and safely remove the USB stick
Boot and test

The USB key now boots directly into FreeDOS and loads into high memory, leaving roughly 600KB of common memory for programs.

[/QUOTE]


STEP2
  
  extract the bios, and copy the DOS folder to the root of USB drive . 
  rename folder to BIOS
  
STEP3 
    
   Boot to USB ( you should of tested it above )
   run biosupdate.bat filename.ext
   this will examine your bios
   create a new autoexec.bat
   and reboot using the new autoexec.bat
   Then it will flash your bios !!!
   
      note this will fail without the flash utility being able to write the autoexec.bat and have clear memory to examing the existing bios and flash the new
      
Enjoy.
   comments ? questions ? post an issue !!
   
