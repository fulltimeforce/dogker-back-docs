---
layout: default
title: Dogker Back
nav_order: 1
has_children: false
parent: Dogker App
---

# Backend
{: .fs-8 .no_toc }
The backend side is mainly built on [NodeJS](https://nodejs.org/en/docs/) and [NestJS](https://nestjs.com/) framework. We are also using Typescript to get a better performance.
{: .fs-6 .fw-300}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Database
We are currently using MongoDB a document-oriented database as well as [mongoose](https://docs.nestjs.com/techniques/mongodb) from NestJS, an Object Data Modeling **(ODM)** library so that we manage our games data.
{: .fs-4 .no_toc }
![Shema](https://i.imgur.com/q62CQsQ.png "Dogker Schema")

### **User**
User is the actual player who enters a game and starts playing. Once a user opens the app it will generate a **code** right away that is going to be stored in **code** attribute. This code was generated to create a room and use it to invite other players as well. As soon as a user enters a room either its own or from another player, it will store the room code in **currentGame**.
{: .fs-4 .no_toc }
{:refdef: style="text-align: center;"}
  ![Shema](https://i.imgur.com/63WMBjp.png "Dogker Schema")
{: refdef}

### **Suggestion**
Suggestion is where we store our suggestions, **forgiving the repitition**. It comes from the [Landing Page](dogker.fulltimeforce.com) when anyone who played Dogker App can send suggestions through a form in there. It will ask a subject which is basically the **title** and the message which is the **suggestion**, all those data will be stored in our collection respectively.
{: .fs-4 .no_toc }
{:refdef: style="text-align: center;"}
  ![Shema](https://i.imgur.com/BVh44bg.png "Dogker Schema")
{: refdef}
## Sockets
We are using sockets which basically allows users/players to send multiple requests to one server. There can be **10 players maximum** in each game whom will send requests continuously such as **betting**, **folding**, **raising bet**, etc. Those methods can be found in [`games.gateway.ts`](https://github.com/fulltimeforce/dogker-back/blob/master/src/games/dto/game.dto.ts)
![Socketio](https://i.imgur.com/5H2mNML.png "Dogker Socketio")

## Project Structure
```
src\
 |--cron\         # Actions to be executed at certain time
 |--deck\    # All related to cards methods
 |--games\           # Mongoose models, gateway and controller
 |--suggestions\    # Mongoose models, gateway and controller
 |--users\         # Mongoose models, gateway and controller
 |--app.module.ts\         # Interaction among all modules from user,game,cron,etc
 |--main.ts\       # App entry point
```