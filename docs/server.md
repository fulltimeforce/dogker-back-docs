---
layout: default
title: Dogker server
has_children: false
nav_order: 3 
---

# Server environment
{: .fs-8 .no_toc }

Dogker server is hosted on an AWS EC2 Instance running Ubuntu 18.04. It is composed of the backend API, media server and landing page.
{: .fs-6 .fw-300}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Backend API

The backend is a NestJS application running on port 5000. The application supports both a REST API and WebSockets communication with the help of Socket.io. You can find it in `var/dogker/dogker-back`. We use [PM2](https://pm2.keymetrics.io/) as a process manager.

## Janus (WebRTC Server)

[Janus](https://janus.conf.meetecho.com/), and more specifically, the [audiobridge plugin](https://janus.conf.meetecho.com/docs/audiobridge.html) have been installed and configured to run on port 8088.

[ICE](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Protocols#ice) and [STUN](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Protocols#stun) are essential in order for a WebRTC connection to be established between two or more people. STUN is needed both for Janus (so the server can find and expose it's own address) and also the clients connecting (Flutter app). We currently make use of Google's public STUN servers for both Janus and the clients. You can find all the configurations in `opt\janus\etc\janus\janus.jcfg` and `opt\janus\etc\janus\janus.transport.http.jcfg`.

WebRTC also requires a HTTPS connection in order to work correctly (needed for microphone detection), so SSL certificates were issued with [Certbot](https://certbot.eff.org/). 

Janus and the audiobridge plugin expose a REST API that we call from the Flutter application in order to manage audio communication. You can refer to the documentation [here](https://janus.conf.meetecho.com/docs/audiobridge).  

## Landing Page

Dogker's [landing page](https://dogker.fulltimeforce.com) was developed with React. You can find it in `var/dogker/dogker-web`.

## Nginx

We use Nginx as a web server and reverse proxy:
 - The root address `https://dogker.fulltimeforce.com` serves the production build of the landing page.
 - Requests made to `/janusbase/` get redirected to the Janus server running on port 8088.
 - Requests made to `/api/` get redirected to the NestJS application running on port 5000.

You can find the configuration in `etc/nginx/sites-enabled/default`.

Note: Currently, `/api/` only works for REST requests such as `POST /api/users/`, but a websockets connection can't be achieved. So, at the moment we use `http://dogker.fulltimeforce.com:5000` as the API address.

