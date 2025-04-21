---
title: 'Real-Time Web Approach and SignalR'
date: 2022-10-11
permalink: /posts/real-time-web-approach/
tags:
  - SignalR
  - Real-Time
  - WebSocket
---

This approach is when the client sends a request to the server and a persistent connection is created between them. It’s mostly used in apps like chat applications, GPS tracking, or gaming.

So how does this work?
======

There were several methods used over time, like: Polling, Long Polling, Forever Frames, Server-Sent Events (SSE), WebSocket, and SignalR.

## 1. Polling
The first idea people came up with was polling. That’s when the client keeps requesting data from the server every certain interval. Basically, it sends an Ajax call at regular time intervals to check if there’s new data.
The problem with this approach was the huge number of requests reaching the server, many of which ended up being useless since there was no new data to return.

## 2. Long Polling
Long polling tried to solve this issue. The client sends a request to the server, and instead of responding immediately, the server holds the request until there’s new data available. Once there’s data, it responds, and the client immediately sends another request, and so on.
However, this had a limitation: according to the HTTP specification, if a request stays open for more than 30 seconds without a response, it triggers a connection timeout error.

## 3. Forever Frames (a Microsoft approach)
In HTTP, responses don’t have to be sent in one go — they can be sent in chunks. These chunks are sent to the browser, where they get assembled and interpreted.
Microsoft found a loophole using chunked encoding — they’d send the first chunk without specifying how many chunks were coming, tricking the client into thinking the connection was still open. Then, every time there was new data, they’d send it as another chunk and these chunks are sent to the browser as scripts to an iframe.

But this approach had several issues:-
- It required complex configurations on the server to send data as response chunks.
- The browser’s interpreter needed to be adjusted to handle single chunks directly.
- It only worked on Internet Explorer.
- Too many script chunks loaded in the page caused performance issues.
- It was one-way — the client could only receive, not send, unless a new connection was made, and that had to be manually handled.

As mentioned, server setup was complex.


## 4. Server-Sent Events (SSE)
With the release of HTML5, a new API called SSE came out, enabling real-time communication. It’s HTTP-based and event-driven, where the server acts as the publisher, raising events and sending data to any subscribed clients when those events occur.

The main drawback was that it was also one-way — if the client wanted to send something, it had to close the connection and reconnect.
But the upside was that reconnections were automatically managed, unlike Forever Frames.
Also, it worked in all browsers except Internet Explorer.


## 5. WebSocket
it’s a protocol built on TCP that enables a full-duplex communication channel between the client and the server.
It was the best option compared to the previous methods in terms of speed.

However, its downside was that it only emerged around late 2011 to early 2012, so most technologies before that didn’t support WebSocket.

## 6. SignalR
A high-level real-time communication framework developed by Microsoft. Built on top of multiple technologies (WebSocket, SSE, Long Polling) to provide fallback support depending on what’s available.

SignalR is made up of several layers:-
* The first layer is the Hub, where you implement your logic functions using C#. From the classes in this layer, SignalR generates proxy classes that clients can interact with.
* The second layer is the Connection Layer. It determines the most suitable method for establishing a connection between the client and server — whether it’s WebSocket, SSE, or Long Polling, in that order, depending on the specifications and capabilities of both the client and the server.