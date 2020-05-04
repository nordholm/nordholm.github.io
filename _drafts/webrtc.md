---
layout: post
title: "WebRTC - Simple API hides tons of complexity and functionality"
---

WebRTC is a great example of how a ton of functionality and complexity can be hidden under a small, relatively simple, API.

I have been working for a while now on building a platform for managing the streaming of cloud-rendered graphical content to browsers and apps over the internet. The content is rendered in Unreal Engine, which has relatively recently (mid-2019) added a "pixel streaming" plug-in that utilizes WebRTC to do its streaming. While the sample web server and scripts provided by Unreal with the plug-in are quite easy to get up and running, and they do a good job for demonstration purposes, they do have limitations, are not intended for production use, and leave a lot to be desired especially in terms of scaling. In addition to this, my project has some additional wrinkles (or huge complex folds) that required their own work. This has provided me the opportunity to work intensely with WebRTC, and I’d like to share some thoughts on this technology.

### What is WebRTC?

[WebRTC](https://en.wikipedia.org/wiki/WebRTC) stands for Web Real-Time Communications and it provides for real-time peer-to-peer streaming of audio and video via a simple API. One of its main intended uses is in browsers to enable video calls between remote peers and to do so without requiring a separate plug-in. WebRTC is free and open source and is supported by the likes of Apple, Google, and Microsoft (to name a few). While it is still working its way through the standardization process of the IETF, it is already support in all major browsers and mobile platforms. It is being used in a number of video conferencing solutions, notably Microsoft Teams.

### Simple API

The JavaScript API that WebRTC exposes to web developers in the browser is deceptively simple. It has just three main entry points:

* getUserMedia() – allows the script to gather information about available devices (microphones, camera, and the screen) and their capabilities. Calling getUserMedia is also what causes the browser to prompt the user for permission to access their devices.
* RTCPeerConnection - TODO
* RTCDataChannel – TODO

Simplest possible example of using the API…

### How does it work?

Streaming is pure peer-to-peer (excepting TURN) using existing protocols, signaling is left to the developer.

...