---
title: RetroArch Tips and Troubleshooting
pageTitle: RetroArch Tips and Troubleshooting - Retroarch - NHL'94 Forums
length: 4624
author: aqualizard
timestamp: 2023-04-16T15:08:07-05:00
markdownload-tags: []
markdownload-source: https://forum.nhl94.com/index.php?/topic/18699-retroarch-tips-and-troubleshooting/#comment-178379
markdownload-hostname: forum.nhl94.com
---

# RetroArch Tips and Troubleshooting - Retroarch - NHL'94 Forums

## Excerpt
> "Tips and Troubleshooting" Cheat Sheet These are some issues and some fixes for RetroArch. The fixes and more details are in the messages below.TROUBLESHOOTING No Audio Issue Win10 Jumpy Video see Issue 8 SNES "Multitap" Netplay Issue Mac Fullscreen/Hotkey Issues fixed in 1.7.9+ "Slow Motion" Vid...

---
**Issue 8: Audio Stuttering and Slow Framerates (PC only)**

This is probably one of the most common issues I hear about - "My audio is stuttering!!!"

**The problem -** Audio stutters because of the video frame rate is incorrect. All retro consoles and console cores on RetroArch strive for a ~60fps frame rate (retro consoles and games are designed to run on 60Hz, which is 60 frames/sec (or close to it - this is NTSC spec, 16.7ms/frame \* 60 frames = ~ 1 second). Your PC/laptop monitor usually runs on 60Hz refresh rate, so the video should be flawless with no slowdown. Simple, this should work perfectly, right? Sometimes yes, sometimes no.....

Even though your monitor's refresh rate is set to 60Hz, Windows has overall control on what to allow programs to run at (via your graphics card's video driver). So, even with a 60Hz refresh rate monitor, and a program that displays video at 60Hz rate, Windows can tell your program to go F itself and only allow it to run at 30Hz (example). This problem is a major one in Windows 8-10, and usually happens with programs that are running in a windowed mode.

**Why is the audio stuttering?**

Well, monitor refresh rates aren't ALWAYS a perfect 60Hz. Depending on many factors (heat is one), the refresh rate fluctuates, very slightly (for example, it might bounce around between 58-61 Hz). This isn't a major problem, because these small changes are not noticeable by eye to us. 

Fortunately, RetroArch monitors this. In order to sync up the audio to the video properly, it "adjusts" the audio to the refresh rate. So, if one minute, the refresh rate is 58Hz, it will slow down the audio to match it up with the slower video rate. If another minute, it's at 61 Hz, it will speed up the audio to sync it with the video. 

The audio stuttering occurs when your refresh rate is something absurdly different (like 50Hz, or 30Hz). Well, now, the audio will sound like absolute crap, even though to you, the video might still be running at an OK speed.

**So, the problem isn't the audio, it's the video.** 

**The solution -** First, we need to check what the actual frame rate is of your monitor. This can be checked in RetroArch, under Settings-Video. You will see 3 important values: **Vertical Refresh Rate, Estimated Screen Framerate,** and **Set Display-Reported Refresh Rate**.

**Vertical Refresh Rate -** This is the rate that RetroArch uses to sync the audio and video. This is 60Hz by default.

**Estimated Screen Framerate -** This is the actual current frame rate of the monitor. This will fluctuate and that is normal (see description above). Though, the fluctuation should be small. This value should be very close to the "Vertical Refresh Rate". If not, you will see slowdown in video and screwy audio. This shows a deviation from the "Vertical Refresh Rate" in a percentage, and also shows a frame count.

**Set Display-Reported Refresh Rate -** This is the refresh rate the your monitor is set to. If you change your monitor's refresh rate in Windows, it will be displayed here. This should be 60Hz (except in very special cases, like high-end gaming monitors). If it isn't set to 60Hz, change it (in Windows) following this guide - [How to change monitor's refresh rate](https://www.howtogeek.com/359691/what-is-a-monitors-refresh-rate-and-how-do-i-change-it/). After you change it, you will need to exit and restart RetroArch.

[![Good Refresh.png][fig1]](https://forum.nhl94.com/uploads/monthly_2019_10/1872146878_GoodRefresh.png.e6d0dbb2de1d7d3403d90422bd4b394c.png)

The important one to look at is "Estimated Screen Framerate". What value is this? Is it close to 60Hz? If so, you can leave it alone, your audio is most likely fine, and you don't have this problem, or something else is causing it.

Many people running older laptops with Windows 8 or 10 will see "Estimated Screen Framerate" in a range of 30Hz-60Hz. This is Windows controlling the frame rate via your video card. Many times, the easiest solution is to play in fullscreen mode (hit the "f" key to toggle fullscreen mode). You should see your "Estimated Screen Framerate" jump up to ~60Hz.

**So, what's the solution? Play in fullscreen mode. If this still doesn't fix the problem, I suggest trying a different video driver (under Settings-Driver). The default is D3D11 (Direct3D 11), but try using the "gl" driver. After changing the driver, you will need to close and restart RetroArch to see an effect. Then, try again, under Settings-Video, looking at "Estimated Screen Framerate", toggling between windowed mode and fullscreen. Also, make sure you are using the "X****Audio" audio driver (Settings-Driver). If you switch to it, and have no sound, look at Issue 1 above for a fix.**

[fig1]: https://forum.nhl94.com/uploads/monthly_2019_10/1157288446_GoodRefresh.thumb.png.63b8799cf3704a2238eec5650638a225.png

> Saved from https://forum.nhl94.com/index.php?/topic/18699-retroarch-tips-and-troubleshooting/#comment-178379 on 2023-04-16T15:08:07-05:00