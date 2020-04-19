---
title: "Increasing Your Ubuntu Partition (Dual Boot with Windows)"
date: 2020-04-19
categories:
- Educational
tags:
- linux
- ubuntu
url: /posts/2020/04/increasing-ubuntu-partition/
---

# Increasing Your Ubuntu Partition (Dual Boot with Windows)

## Why Dual Boot?
I've been dual-booting Windows and Linux for nearly a decade now. I couldn't afford a fancy Macbook for Unix-based development as is ubiquitous these days, so getting a cheap Windows machine and dual-booting Linux on it was my best option. (It is a universal truth that VMs are god-awful for developing in.)

These days, there are more options than ever for cross-platform development:
- Microsoft introduced [WSL](https://docs.microsoft.com/en-us/windows/wsl/about), a subsystem for Linux a couple years ago. It looks pretty freaking cool, although I haven't used it much beyond some cursory Bash scripts.
- Thumbdrive mounts. These are easy to lose, so not a great option for a permanent dev setup. I have an old Kali thumbdrive set up for pentesting that works well for plug-and-play purposes.
- If you just want to compile and run code, there are plenty of free websites that let you do that online now. `Repl.it` and `ideone` are a few. 

Dual booting can be a pain to set up (my current Dell XPS laptop has very annoying driver and BIOS issues that were annoying to get around when installing Ubuntu). I personally find it worth it, since I use Windows primarily for gaming and Linux for coding/hardware hacking/everything else. That being said, I also get the advantage of using a free Macbook for my full-time development job. Macbooks are a great option for development if you can afford it, so if you don't need Windows or Linux for something particular you're probably better off just getting a Macbook. 

A consistent issue I (and many others) run into with dual booting is misestimating how much size the Linux partition will need. I get a new laptop or desktop every few years, and my needs for programs change with time. So it's common to need to shrink or grow your Linux partition at some point. I just went through the process of increasing the Ubuntu partition on my laptop, so that's what this blog post focuses on. However, the process is similar for shrinking as well.

## How To Increase Your Ubuntu Partition Size
### Backup
You should be safe and backup Windows and Ubuntu first. I have a lot of experience with dual booting, so I threw caution to the wind and didn't back anything up this time. It worked out fine but things can happen, so at the very least make sure any important files are saved to the cloud first, especially in your Ubuntu partition.

### Create Unallocated Space in Windows
First, make sure you have unallocated space available. You'll want to do this in Windows (with dual boots, Windows is always the "primary" because it is very greedy and doesn't like to share) so boot into that.
- Use Disk Management to free up space by shrinking a volume. I put about 30GB out of the ~250 I had on my main Windows partition, into unallocated space. (Anyone reading this is presumably already familiar with how to do this from when you initially set up the dual boot, using a process such as https://itsfoss.com/install-ubuntu-1404-dual-boot-mode-windows-8-81-uefi/).

### Grow Your Linux Partition in Ubuntu
To grow your Linux partition, you now need to reboot into Ubuntu. 
 But not the normal mount - instead, boot up from the live USB/disc you used to install Ubuntu. You can't do the next step on your normal Ubuntu partition even if you install GParted onto it; GParted won't allow you, because it could wreak havoc if you repartitioned from within the mounted volume.
- From within the "trial Ubuntu", use GParted to add the additional space, that you unallocated in Windows, to your Ubuntu partition. Identify the partition, right click, hit Resize/Move, and drag the slider to take up the unallocated space. Then just hit the green checkmark to apply the operation.
- My resizing operation initially failed with "no such file or directory" from fdisk; a quick search brought up this answer (https://unix.stackexchange.com/a/434420). I ran `partprobe` in a terminal, retried the resize operation, and it worked.

Howtogeek has a pretty good step-by-step guide with pictures for doing this part of the process: https://www.howtogeek.com/114503/how-to-resize-your-ubuntu-partitions/

## Takeaway
Dual booting can be a great option, depending on your operating system needs. Resizing your Linux partition takes a little extra care, but it's not difficult.