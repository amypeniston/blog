---
layout: post
title: "BSoDs and BIOS Flashing"
modified:
categories: 
excerpt: Out-of-date drivers cause errors with Operating System updates. Here's how I updated my BIOS to fix a Blue Screen of Death for Windows 10 Anniversary Edition.
tags: [windows 10, updates, bios flashing, troubleshooting, tech]
image:
  feature: 2016/08/bsod-header.jpg
date: 2016-08-11T09:13:58-03:00
---

August 2nd, 2016: I had my stomach drop out the other day when I did an update/shut down for Windows 10 Anniversary Edition and woke up the next morning to a Blue Screen of Death: Critical Process Died Error. 

Shit. Worst feeling in the world.

I followed on-screen instructions to reboot while Windows attempted to "fix the problem" and got caught in a reboot loop. Still BSoD with a message saying "Windows did not load correctly". 

Here's how I worked through what was going on and, by an incredible stroke of good fortune, figured out a solution. 

**TLDR: Revert to previous build to go back to before the update was installed. Download and use BlueScreenView to investigate the Windows error minidump file and determine which file/driver caused the error. In my case ntoskrnl.exe indicated a problem with either RAM or BIOS. Enter your BIOS configuration menu by pressing DEL during reboot. Check your BIOS version, it could be out of date (mine was). If so, download the latest software version from the manufacturer's website, load it on a blank, bootable flash drive and use the M-FLASH tool in the config menu to update your BIOS.**

## Revert to Previous Build

After the initial BSoD I was stuck in a reboot loop where no amount of restarting fixed anything. I tried entering the Command Prompt from the Troubleshooting menu and running a sfc /scannow. This produced a "Windows Resource Protection could not perform the requested operation." The only other thing I could do was revert back to a previous build. 

This at least enabled me to enter Windows again, however I knew that in a day or two the computer would prompt me to update and, in all likelihood, would BSoD again. Lo and behold on August 6th, the Anniversary update was available for download. I selected Update/restart and - same error. 

Once more I reverted to previous build, with a sinking feeling of doom.

## Driver Updates

The first recommendation I came across was to update drivers - every last one of them. I entered Device Manager, right clicked on each component and checked to see whether any updates were available. None were, except for my Graphics driver, which I updated via the NVIDIA control panel. Still, I was very doubtful that a minor Graphics driver version would break the Windows update process so I knew I had to keep looking for another underlying issue.

## BlueScreenViewer

Lots of tech forum trawling followed. I found mention of [BlueScreenView](http://www.nirsoft.net/utils/blue_screen_view.html) and decided to give it a try. Once downloaded and run, the program reads the otherwise unintelligible minidump error file that is created when Windows dies for one reason or another. BlueScreenView indicated that my problem was due to the ntoskrnl.exe driver. Googled that shit and all signs pointed to either a RAM or a BIOS problem. It dawned on me that I hadn't checked my BIOS version. 

## BIOS Config: MSI Z87-G45 Gaming

I rebooted my system and entered BIOS config by pressing the DEL key while restarting. I see version 1.2 at the top of the screen. Headed over to MSI's website, to the MSI Z87-G45 Gaming downloads page and - WOAH. Current version is 1.9. With almost 2 years of updates between what I was running and stable release I knew I had found something. Next, update BIOS.

## DO NOT USE MSI'S LIVE UPDATE 6 TOOL

So MSI has an "easy" live update tool that is supposed to cut out any complicated BIOS flashing procedures by providing a simple download GUI. Just as I was about to try this I decided to verify that it worked as intended. BAD. NO NO NO. Pages and pages of reviews describing how the live update doesn't account for background processes and bricks almost every system it weasles its way into. Dodged that bullet.

## Flashing the BIOS

Better do it the old fashioned way (see [this BIOS flashing video by MSI](https://www.youtube.com/watch?v=LRyFMf0D9Lc)). Luckily I had a flash drive on hand that I could wipe, using the right click>Properties>Restore Factory Defaults option. Once clean I unzipped the BIOS 1.9 driver to the root of the drive and right clicked>Ejected it (but left it in the back of the machine). 

Then I rebooted, entered BIOS config with DEL, and selected the M-FLASH tool in the lower left. This prompted me to select the flash drive with the driver software. Stepped away while the display flashed and the system rebooted (and by stepped away I mean held my breath).

The update worked, as shown at the top of BIOS config with the new version number. 

## Trying the Update Again

A few days later, on August 10th, 2016, Windows indicated an update was available. This time it worked! Hopefully at least one other desperate sod in a similar boat reads this and is able to figure out a solution. Ta ta for now.

## Other Random Notes

* During my digging I read time and again that Daemon Tools can cause issues with Windows updates. If you have that suite on your computer, try uninstalling prior to the update.

* Create a system restore point via Control Panel to revert to previous moments in your computer's history. When I first got the BSoD I was unable to revert from the Troubleshooting menu because I hadn't set up system restore before the error occurred. You better believe the first thing I did when I got back into Windows was set one up.