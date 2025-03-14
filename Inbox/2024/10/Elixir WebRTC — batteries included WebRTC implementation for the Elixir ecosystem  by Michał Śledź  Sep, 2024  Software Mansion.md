---
created: 2024-09-12T17:35:37 (UTC -03:00)
tags: []
source: https://blog.swmansion.com/elixir-webrtc-batteries-included-webrtc-implementation-for-the-elixir-ecosystem-249eff4073d8
author: Michał Śledź
---

# Elixir WebRTC — batteries included WebRTC implementation for the Elixir ecosystem | by Michał Śledź | Sep, 2024 | Software Mansion

> ## Excerpt
> All you need to build fully functional, real-time applications in Elixir.

---
[

![Michał Śledź](https://miro.medium.com/v2/resize:fill:44:44/1*JbPLojcJQhnSB_bUSoFyCA.png)



](https://medium.com/@michal.sledz?source=post_page-----249eff4073d8--------------------------------)[

![Software Mansion](https://miro.medium.com/v2/resize:fill:24:24/1*5QvEwVc_cxdJHcepbaRHhw.jpeg)



](https://blog.swmansion.com/?source=post_page-----249eff4073d8--------------------------------)

![](https://miro.medium.com/v2/resize:fit:875/1*o8V-ksZJxbECF8saiWeYpA.png)

Last year, together with [Łukasz Wala](https://medium.com/u/004caee04370), we started a brand new project called [Elixir WebRTC](https://github.com/elixir-webrtc). The initial goal was straightforward — let’s try to create a project similar to [Pion](https://github.com/pion) but for the Elixir community. However, we realized quickly that bringing just WebRTC API won’t be enough. It’s too low-level and too complex, and the learning curve is steep. With that, our overarching goal became to bring WebRTC to the masses. We wanted to provide people with the whole ecosystem — all the pieces you need to create a fully functional application without a hassle. And building such a system is a lot of work.

The final requirements we defined for Elixir WebRTC were to:

-   mimic JS API as much as possible,
-   be easy to integrate with other Elixir frameworks and libraries — Phoenix, Membrane, Elixir Nx,
-   provide everything that’s needed to work with audio and video data without the need to use third-party multimedia libraries,
-   include learning resources and guides.

In other words, we wanted to enable exploring WebRTC just by playing with Elixir WebRTC.

Let’s see what it means! :)

## Discoverability

When you enter a new technology, one of the first questions is: “What can I build with it?”. Well, Elixir WebRTC comes with a [website](https://elixir-webrtc.org/) that aims to show you exactly that — demo applications deployed 24/7. They include:

-   [Recognizer](https://recognizer.elixir-webrtc.org/) — a simple real-time image recognition app. Uses Phoenix Framework, Elixir Nx, and Elixir WebRTC.
-   [Broadcaster](https://broadcaster.elixir-webrtc.org/) — a WHIP/WHEP, Twitch-like streaming service. The deployed version runs an infinite stream so you can use it to test your WHEP clients.
-   [Nexus](https://nexus.elixir-webrtc.org/) — a single-room, Google Meet-like, video conferencing app.
-   [Rel](https://github.com/elixir-webrtc/rel.git) — standalone, UDP TURN server able to handle gigabits/s of data traffic.

The icing on the cake is a live coding video presenting how to use our API.

## Observability

The next thing is observability. Whenever you are on a Google Meet, Discord, or any other WebRTC-based meeting running in your browser, you can go under chrome://webrtc-internals and see a lot of plots and metrics.

![](https://miro.medium.com/v2/resize:fit:875/1*YWxTDFyrZ_BKRDBqvgo4zA.gif)

chrome://webrtc-internals

Those metrics include packets per second you are sending/receiving, the number of video freezes, frames dropped, or keyframe requests, the video quality limitation reason, what codecs are used, and so on.

The tool is extremely useful for debugging but it only runs in a web browser on the client side, which sometimes is not enough.

In Elixir WebRTC and Phoenix, you can get the same thing but for the server side just by adding two lines of code! See the video.

## Learning Resources

Documentation is king in Elixir — the same goes for Elixir WebRTC. We created [a series of tutorials](https://hexdocs.pm/ex_webrtc/readme.html) introducing you to both WebRTC and Elixir WebRTC. The idea is to limit the number of external resources you have to read before starting your journey. Our guides include:

-   how JS API maps to the Elixir’s one
-   what are transceivers and how to use them
-   how to deploy WebRTC apps
-   how to debug WebRTC apps

and many, many more!

![](https://miro.medium.com/v2/resize:fit:800/1*uvcU1Td3-w2doB7jmqe4Jg.gif)

Elixir WebRTC Tutorials

## WHIP/WHEP ready

WebRTC adoption started by creating video conferencing systems but nowadays it is moving strongly towards real-time streaming. More and more companies are trying to scale their WebRTC solutions to thousands of viewers. To make this process more standardized, two more acronyms emerged — WHIP and WHEP. They are very tiny protocols built on top of WebRTC and are meant for broadcasting.

Making this less abstract — WHIP is a protocol that OBS can use to send a stream to a broadcasting server and WHEP is a protocol that web browsers can use to receive this stream.

Elixir WebRTC is fully compatible with both WHIP and WHEP. We even implemented them in our [Broadcaster app](https://github.com/elixir-webrtc/apps/tree/master/broadcaster) and this [example](https://github.com/elixir-webrtc/ex_webrtc/tree/master/examples/whip_whep). Watch the below video to see how you can stream from OBS to the Phoenix-based app :)

Streaming from OBS to the Phoenix-based app

Streaming from OBS to the Phoenix-based app

## Enabling AI

Ahh, there is no project that doesn’t “use AI” today, right? To meet this need, we created [Xav](https://github.com/elixir-webrtc/xav.git) — a tiny wrapper around FFmpeg for decoding audio and video so you can easily feed it into [Elixir Nx](https://github.com/elixir-nx/nx). For example, converting an encoded video frame into Nx tensor is as easy as creating a new decoder and calling two functions:

```
<span id="bd6b" data-selectable-paragraph="">decoder = Xav.<span>Decoder</span>.new(<span>:vp8</span>)<br>{<span>:ok</span>, %Xav.Frame{} = frame} = Xav.Decoder.decode(decoder, &lt;&lt;<span>"somebinary"</span>&gt;&gt;)<br>tensor = Xav.Frame.to_nx(frame)</span>
```

## Our older brother Membrane

We didn’t forget about our older brother Membrane or to be more specific, the Membrane Team didn’t forget about us and created Sink and Source elements that use Elixir WebRTC. Check them out [here](https://github.com/membraneframework/membrane_webrtc_plugin)!

## What’s next

For the most inquisitive, here is the full list of features currently implemented in Elixir WebRTC:

-   full ICE with support for TURN servers
-   RTX , TWCC, RTCP reports
-   Simulcast — multiple inbound resolutions
-   Opus/VP8 payloaders and depayloaders
-   IVF and Ogg containers
-   getStats() API
-   DataChannels (in the next release)

Now, we turn our attention to bringing Erlang Distribution into Elixir WebRTC. Our first goal is to distribute the Broadcaster app and allow the creation of any size cluster for real-time streaming. Stay tuned!

We’re Software Mansion: software development consultants, AI explorers, multimedia experts, React Native core contributors, and community builders. Hire us: [projects@swmansion.com](mailto:projects@swmansion.com).
