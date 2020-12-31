---
layout: post
title: "Digital Humans in 1995"
---

_"Digital Humans"_, I thought, _"Wasn't that also the name of a piece of software that I bought back in the 90's?"_ So I went rummaging through a box of old computer stuff: obsolete cables and connectors, CD-ROMS and floppy disks - the usual things I keep for no particular reason. And there it was, a CD-ROM from 1995 titled _"Digital Humans"_.

If I remember correctly, I had read an article about this in a print magazine and found it intriguing . I ordered a copy via snail mail with a cheque included and received the CD-ROM in the mail some weeks later. How things have changed in a quarter century! Apart from the title, of course this is completely unrelated to what we call Digital Humans today. Nonetheless, this is a very cool technology demonstrator from that time.

![Digital Humans CD-ROM Title Screen](/images/DigitalHumans_title.png)

## About the software
The CD-ROM contains multimedia software published as part of the [Visible Human Project](https://www.nlm.nih.gov/research/visible/visible_human.html). This project "translated a male and a female human body into unlabelled digital atlases, using both color as well as x-ray and magnetic resonance imaging". The images were generated from the cadavers of two recently deceased individuals who had donated their bodies to science. The x-ray and magnetic resonance images were produced in the usual non-invasive manner, while the color images were produced in a novel - and destructive - way. The frozen cadavers were milled away in 1 mm slices for the male and 0.3 mm slices for the female, and the revealed cross-sections were photographed between each milling step, thus enabling the construction of a fairly high-resolution 3D photogrammetric model of the cadavers to complement the x-ray and MR scans. The software on the CD-ROM allows the user to interactively navigate these 3D images and also includes a lot of interesting video information about the preparations and image processing performed in the Visible Human Project.

Here's a short video demonstration of the running software (including the rather satisfying Windows 3.1 start-up and shut-down sounds):

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/vrFKivODdNw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

I am including some images from the CD sleeve which contain some historically amusing information. You can open the images in a new tab or download them to see them in full resolution.

| ![Digital Humans CD-ROM Front](/images/DigitalHumans_sleeve_front.png){: width="360px"} | ![Digital Humans CD-ROM Back](/images/DigitalHumans_sleeve_back.png){: width="360px"} |

![Digital Humans CD-ROM Inner](/images/DigitalHumans_sleeve_inner.png)

Note the included 3D glasses! Some of the interactive content is rendered stereoscopically.

| ![Digital Humans CD-ROM Inner Sleeve 1](/images/DigitalHumans_sleeve_1.png){: width="360px"} | ![Digital Humans CD-ROM Inner Sleeve 2](/images/DigitalHumans_sleeve_2.png){: width="360px"} |


## Getting it running

Getting this software up and running was a bit of a nostalgia trip.

I started by borrowing my wife's desktop computer, which luckily still has a DVD/CD-ROM drive installed, in order to copy the contents of the CD onto a flash drive. Because this is 16-bit software that was designed to run on Windows 3.1 or Windows NT, it cannot be run directly in Windows 10. Some sort of virtualization or emulation was required.

I first attempted to install the software into a Linux Docker container with the [Wine](https://en.wikipedia.org/wiki/Wine_(software)) Windows compatibility layer. I was hopeful when the installer started up correctly and let me choose my installation options. But my hopes were dashed when, shortly after the actual installation began, the installer crashed.

For my second attempt, I followed [this tutorial](http://www.sierrahelp.com/Utilities/Emulators/DOSBox/3x_install.html). I downloaded and installed [DOSBox](https://www.dosbox.com/) which is a DOS emulator that allows you to run old DOS-based programs on modern PCs and operating systems. On top of DOSBox I installed Windows 3.1 from a set of 6 floppy disk images. In order to display more than 16 colours in Windows 3.1, I then installed an updated graphics driver. To get audio output, I installed SoundBlaster drivers. Before long, I had Windows 3.1 running in an application window on my Windows 10 desktop! Installing the Digital Humans software into this environment was straightforward and, as you can see from the video above, it works!

| ![Digital Humans CD-ROM Windows Install Screen 1](/images/DigitalHumans_win_install_1.png){: width="360px"} | ![Digital Humans CD-ROM Windows Install Screen 2](/images/DigitalHumans_win_install_2.png){: width="360px"} | ![Digital Humans CD-ROM Windows Install Screen 3](/images/DigitalHumans_win_install_3.png){: width="360px"} |

The software crashes occasionally. I'm not sure if this is due to the emulation environment or if the software was always a little unstable. Regardless, revisiting this software was a fun little holiday project.

![Digital Humans CD-ROM Credits Screen](/images/DigitalHumans_credits.png)
