---
layout: post
title: "Digital Humans in 1995"
---

I recently went through a box of old computer stuff and stumbled on a CD-ROM from 1995 titled _"Digital Humans"_. If I remember correctly, I had read an article about this in a print magazine. So I sent my order via snail mail with a cheque included and received the CD-ROM in the mail some weeks later. Much has changed in a quarter century! Apart from the title, this is, of course, completely unrelated to what we call Digital Humans today. Nonetheless, this is a very cool technology demonstrator from that time.

## About the software
The CD-ROM contains multimedia software published by [Multimedia Medical Systems]() and is part of the [Visible Human Project](). This project "translated a male and a female human body into unlabelled digital atlases, using both color as well as x-ray and magnetic resonance imaging". The images were generated from the cadavers of two recently deceased individuals who had donated their bodies to science. The x-ray and magnetic resonance images were produced in the usual non-invasive manner, while the color images were produced in a novel - and destructive - way. The frozen cadavers were milled away in 1 mm slices for the male and 0.3 mm slices for the female, and the resulting cross-section was photographed between each milling step, thus producing photographic cross-sections of each cadaver as fairly high resolution. The software on the CD-ROM allows the user to interactively navigate these 3D images and also includes a lot of interesting video information about the preparations and image processing performed in the Visible Human Project.

I am including some images from the CD sleeve which contain some historically amusing information. Below the images is a short video demonstration of the running software that I have put together.

TODO images and video

## Getting it running

Getting this software up and running was a bit of a nostalgia trip.

I started by borrowing my wife's desktop computer, which luckily still has a CD-ROM drive installed, in order to copy the contents of the CD onto a flash drive. Because this is 16-bit software that was designed to run on Windows 3.1 or Windows NT, it cannot be run directly in Windows 10. Some sort of virtualization or emulation was required.

I first attempted to install the software into a Linux Docker container with Wine to emulate the Windows layer. I was hopeful when the installer started up correctly and let me choose installation options. But my hope was shattered when, shortly after the actual installation began, it crashed.

For my second attempt, I followed [this tutorial](http://www.sierrahelp.com/Utilities/Emulators/DOSBox/3x_install.html). I downloaded and installed [DOSBox]() which is a DOS emulator. On top of DOSBox I installed Windows 3.1 from a set of 6 floppy disk images. In order to display more than 16 colours in Windows 3.1, I then installed an updated graphics driver. To get audio output, I installed SoundBlaster drivers. Before long, I had Windows 3.1 running in an application window on my Windows 10 desktop! Installing the Digital Humans software into this environment was straightforward and, as you can see from the video above, it works!

The software crashes occasionally. I'm not sure if this is due to the emulation environment or if the software was always a little unstable. Regardless, revisiting this software was a fun little holiday project.

---

Trying with DOSBox with Windows 3.1:
* Windows on DOSBox tutorial - http://www.sierrahelp.com/Utilities/Emulators/DOSBox/3x_install.html
* Win31 download - https://archive.org/details/Windows3.1_201906
* 7-zip - https://www.7-zip.org/

Windows installed in just a few minutes. To run Windows:
* Start DOSBox
* mount c c:\temp\dighuman
* run c:\AUTOEXEC.BAT
* run WIN

Also needed to install >16 colour Tseng drivers and SoundBlaster drivers!


