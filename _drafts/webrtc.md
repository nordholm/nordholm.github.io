---
layout: post
title: "WebRTC - Simple API hides tons of complexity and functionality"
---

WebRTC is a great example of how a ton of functionality and complexity can be hidden under a small, relatively simple, API.

I have been working for a while now on building a platform for managing the streaming of cloud-rendered graphical content to browsers and apps over the internet. The content is rendered in Unreal Engine, which has relatively recently (mid-2019) added a "pixel streaming" plug-in that utilizes WebRTC to do its streaming. While the sample web server and scripts provided by Unreal with the plug-in are quite easy to get up and running, and they do a good job for demonstration purposes, they do have limitations, are not intended for production use, and leave a lot to be desired especially in terms of scaling. In addition to this, my project has some additional wrinkles (or huge complex folds) that required their own work. This has provided me the opportunity to work intensely with WebRTC, and I’d like to share some thoughts on this technology.

### What is WebRTC?

[WebRTC](https://en.wikipedia.org/wiki/WebRTC) stands for Web Real-Time Communications and it provides for real-time peer-to-peer streaming of audio and video via a simple API. One of its main intended uses is in browsers to enable video calls between remote peers and to do so without requiring a separate plug-in. WebRTC is free and open source and is supported by the likes of Apple, Google, and Microsoft (to name a few). While it is still working its way through the standardization process of the IETF, it is already supported in all major browsers and mobile platforms. It is used in a number of video conferencing solutions, notably Microsoft Teams.

### Simple API

The JavaScript API that WebRTC exposes to web developers in the browser is deceptively simple. It has just three main entry points:

* getUserMedia() – allows the script to gather information about available devices (microphones, camera, and the screen) and their capabilities. Calling getUserMedia is also what causes the browser to prompt the user for permission to access their devices.
* RTCPeerConnection - TODO
* RTCDataChannel – TODO

Simplest possible example of using the API…
{code}

It should be apparent to any programmers reading this that this code is incomplete. While this is the core of the JavaScript code required to start up a WebRTC connection between two peers, something is missing. Before this can work, some form of communication channel must be established between the peers, so that they can exchange information about exactly how the streaming connection can be established. This is referred to as *signalling*. The WebRTC standard is deliberately agnostic about how this is accomplished. In principle, signaling messages could be transmitted by carrier pigeons or smoke signals. More often, signaling is handled via an intermediary such as a web site that both peers connect to, and through which they exchange signaling messages. 

The specifics of the signaling messages is one place where it becomes apparent that WebRTC builds on a host of pre-existing technologies and standards. [Interactive Connectivity Establishment](https://en.wikipedia.org/wiki/Interactive_Connectivity_Establishment) is used to figure out how the peers can communicate directly at the network level, while [Session Description Protocol](https://en.wikipedia.org/wiki/Session_Description_Protocol) is used to convey information about which media streams each peer supports, i.e. which audio and video formats and codecs they are able to transmit and receive. 

A typical signaling exchange takes the following form, where one peer has the initiating role (analogous to the party making a telephone call) and the other peer has the receiving role (analogous to the party picking up the phone when it rings).

A  ->   offer    ->  B
B  <-   answer   <-  B
A  ->  ICE cand  ->  B
A  <-  ICE cand  <-  B

Note that these messages are usually exchanged via an intermediary, and they allow the peers to negotiate a direct peer-to-peer connection, if possible. If it is not possible to establish a direct peer-to-peer connection because of one or both parties' network configuration, it is possible to use a relay server as an intermediary during the streaming as well - a so-called TURN server. TURN stands for "Traversal Using Relay NAT". Once the peer-to-peer connection has been established, the peers can stream media and data to each other using this connection.

At this point the signaling intermediary is effectively done. However, for many practical application such as multi-party video conferencing, each peer will maintain its connection to the intermediary so that it can be notified of things like additional peers joining the conference, etc. Likewise there are other potential issues with many-to-many connections where the intermediary has a role to play...

### How does it work?

Streaming is pure peer-to-peer (excepting TURN) using existing protocols, signaling is left to the developer.

...