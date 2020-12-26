---
layout: post
title: "WebRTC - a brief overview"
---

In my work on digital humans, I have been working for a while now on a platform for managing the streaming of cloud-rendered avatars to users of browsers and apps over the internet. In this solution, content is rendered in Unreal Engine which in mid-2019 gained a "pixel streaming" plug-in that utilizes WebRTC to handle streaming of game content to clients over the internet. This has provided me the opportunity to work intensely with WebRTC, and I’d like to share some thoughts about this technology.

## What is WebRTC?

[WebRTC](https://en.wikipedia.org/wiki/WebRTC) stands for Web Real-Time Communications. It provides for real-time peer-to-peer streaming of audio and video via a relatively simple API. One of its main intents is to enable video calls between remote users in true peer-to-peer fashion using only their browsers and without requiring them to install a browser plug-in or struggle with firewall settings. WebRTC is free and open source and is supported by the likes of Apple, Google, and Microsoft (to name a few). While it is still working its way through the standardization process of the IETF, it is already supported in all major browsers and mobile platforms. Furthermore, it is already in wide use in a number of video conferencing solutions, notably Microsoft Teams, Google Meet, GoToMeeting, and others. WebRTC is certainly here to stay.

When establishing a streaming connection between two parties, the ultimate goal is for the parties to stream audio, video, and data directly to one another without going through an intermediary. This serves the dual purposes of 1) reducing total bandwidth and compute usage and 2) reducing the latency of the connection for a better "live" experience. However, establishing a peer-to-peer connection over the internet is a non-trivial matter.

### Signaling

Normally one or both peers are behind firewalls designed to protect them from receiving messages directly from other devices on the internet. Mostly the only messages that are allowed in through the firewall are responses to the requests that the peer itself has made. The most well-known example is the content received by your browser in response to requests made to a web server. The messages that are allowed back in through your firewall to the browser all arrive in response to the browser sending a message to the web server saying "please send me this content". Imagine if any website could freely send data to your browser without being asked. You would probably be staring at thousands of browser tabs full of nothing but ads!

The reason that websites can receive your requests for content is that they have been deliberately configured to receive such requests from the internet at large specifically for this purpose.

One key feature of WebRTC is that it hides the intricacies of establishing peer-to-peer connectivity from the application developer, so they don't have to solve this part of the equation themselves. Under the covers, WebRTC relies on a number of existing standard protocols to do the heavy lifting.

WebRTC uses [Interactive Connectivity Establishment (ICE)](https://en.wikipedia.org/wiki/Interactive_Connectivity_Establishment) to establish a peer-to-peer connection. Peers use a [Session Traversal Utilities for NAT (STUN)](https://en.wikipedia.org/wiki/STUN) server to discover their public IP addresses, that is their addresses as seen from outside their respective firewalls. This information is then shared in ICE messages sent to the other peer. Using this shared information they are often able to establish peer-to-peer communications through their respective firewalls by using "hole-punching" - which is not as sinister as it sounds - to signal to their firewalls that they are temporarily willing to receive messages directly from the other peer.

In some cases it is impossible to establish direct connections, and the parties must fall back to using a [Traversal Using Relays around NAT (TURN)](https://en.wikipedia.org/wiki/Traversal_Using_Relays_around_NAT) server to relay the streaming data between them. Often a single server supports both STUN and TURN functionality.

"_Wait a minute_", you may be thinking, "_if the peers cannot initially exchange messages directly, then how do they exchange ICE messages?_" ICE messages are usually exchanged via an intermediary such as a website or other service that both parties connect to for this exact purpose. In this context the intermediary is often referred to as a *signaling server*, and the exchange of ICE messages as *signaling*. The WebRTC standard does not prescribe how signaling is accomplished, which is deliberate in order to provide as much flexibility as possible. In principle, signaling messages could even be transmitted by carrier pigeons or smoke signals.

Another important standard used as part of WebRTC signaling is [Session Description Protocol (SDP)](https://en.wikipedia.org/wiki/Session_Description_Protocol). SDP messages describe the types of media (e.g. video, audio, and/or data) that each peer wants to send and receive and which media encodings and formats they are capable of using. Each peer also generates and includes cryptographic keys for one-time use in order to enable end-to-end encryption of streaming data.

Once SDP and ICE have done their job the peers now have a path to start streaming data to one another. At this point yet another standard, [Real Time Streaming Protocol (RTSP)](https://en.wikipedia.org/wiki/Real_Time_Streaming_Protocol), is used to actually send and receive multimedia data over the established connection. I won't get into the many details of RTSP as that could easily fill a separate article, but suffice it to say that this takes care of things like optimizing data streams for the available bandwidth, retransmission of lost data, and many other essentials of successful streaming.

**TODO** simple diagram with 2 peers and a signaling server? Include labels showing where ICE and SDP and RTSP are used.

### Simple API

At this point you probably have the impression that WebRTC is a quite complex beast supported by many additional standards and protocols and that using WebRTC in your own development projects might be rather daunting. Certainly building something comparable from scratch would be a monumental undertaking. But how difficult is it to work with WebRTC as a developer?

The JavaScript API that WebRTC exposes to web developers in the browser is deceptively simple. It has just three main entry points:

* navigator.mediaDevices.getUserMedia() – gathers information about available devices (microphones, camera, and the screen) and their capabilities while prompting the user for permission to access these devices
* RTCPeerConnection – manages the connection to the peer, including the actual audio and video streams
* RTCDataChannel – manages data channels to exchange custom messages (not audio and video) with the peer via the RTCPeerConnection

The bulk of the interaction with a custom web application will be with RTCPeerConnection. It has a multitude of methods and callbacks that must be orchestrated with both the frontend code and the signaling server. However, it does all the heavy lifting associated with ICE, SDP, and RTSP, so that you as a developer can concentrate on your application-specific functionality while getting streaming audio and video almost for free. The RTCPeerConnection API produces and consumes all the signaling messages for you – all the application developer has to do is manage the transmission of these message between peers.

I don't want to clutter this post by including code examples, but suffice it to say that there are functioning examples of websites offering simple WebRTC calls between connected clients that are written in only a few hundred lines of JavaScript. WebRTC APIs are also available for app developers on Android and iOS, and these APIs closely resemble the browser API. This makes it about as easy to incorporate WebRTC in a mobile app as in a web page.

This out of the box simplicity is great when you are developing apps or websites for streaming between users on different frontend devices. But what if you also need a server side WebRTC peer?

## WebRTC and digital humans

Initially, the digital human scenario may look more like a client/server scenario than a peer-to-peer scenario. After all, rather than connecting to another user who is physically located somewhere else in the world, a user connects to a service on the internet in order to interact with an avatar. However, this is easily viewed as a peer-to-peer solution by considering the avatar not as a service but as a peer on an equal footing with the human user.

While the sample web server and scripts provided by Unreal with their pixel streaming plug-in are quite easy to get up and running, and they do a good job for demonstration purposes, they are not directly suited to the digital human scenario. The sample scripts seem to be designed mainly to allow multiple spectators to view a game being played by a single player, while the digital human scenario calls more for a one-to-one interaction between a human and an avatar. Additionally, the scripts do not address issues of scaling to 10s, 100s, or even 1000s of such simultaneous interactions.

More importantly, the digital human must be able to see and hear the user, and the pixel streaming plug-in does not support the streaming of audio and video from the user to Unreal. Our solution side-steps this limitation by using a separate server-side component to receive the audio and video from the user and perform Voice Activity Detection (VAD) and subsequently speech recognition and emotion detection. Basically, two separate WebRTC connections are established, one between Unreal and the user and the second between the user the VAD component. On the server side, the VAD component is able to communicate directly with Unreal, because they don't have to contend with the limitations otherwise imposed by firewalls.

Implementing the server side VAD component introduced an additional challenge. The WebRTC APIs described above are designed to run within their respective client frameworks either as part of a mobile app or within a browser. They are therefore not applicable in a server setting. Luckily there are libraries available written in a variety of server side programming languages such as [node-webrtc](https://github.com/node-webrtc/node-webrtc) for NodeJS and [aiortc](https://github.com/aiortc/aiortc) for Python.

However, the most feature complete and high-performance library I have found is [GStreamer](https://gstreamer.freedesktop.org/). GStreamer is a multiplatform multimedia framework written in C. It has solid language bindings for most popular programming languages and includes a feature-rich WebRTC plug-in. This means that it can be used in a variety of settings including servers and embedded devices.


## Summary

WebRTC is a great example of how a ton of functionality and complexity can be hidden behind a small, deceptively simple API. As is often the case, the devil is in the details, and moving beyond basic examples quickly forces you to consider those details.

