---
layout: default
title: How does it work?
has_children: false
nav_order: 2 
---

# How does it work?
{: .no_toc } 

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## The Game

The game is played through a WebSocket connection between the players and the server. The connection lifecycle and events are defined in `games.gateway.ts`. The gateway makes use of Socket.io for socket management. A game is composed of a minimum of 2 players and a maximum of 10.

The actual game logic is defined in `game.dto.ts`. Logic related to game management is defined in `games.service.ts`. There is also a couple of utility REST functions defined in `game.controller.ts` to obtain valid game codes and find if a player is in a specific game.

Every dogker game is tied to a `gameCode`. This is the random, unique alphanumeric code assigned to players when they start the application. This code also defines the socket room and audio room to be used. Every dogker game in progress is stored in-memory in a dictionary defined in `games.service.ts`, where the key is the `gameCode` an the value is the whole game object. This data store is cleaned after a game ends, and there is also a CRON service `cron.service.ts` that runs every day at midnight to clean the remaining games if there is any left-overs.

### Deck of cards

Logic related to handling the deck of cards for a game is defined in `deck.service.ts`. A deck of cards is an array of strings going from `1-c` (Ace of clubs) to `13-s` (King of spades). Here we handle creation of a deck, shuffling the array using the popular Knuth shuffling algorithm, and labeling of the cards.  

## The Players

Every player, hereinafter called user, is assigned a `gameCode`. When the user creates a game, he also creates an audio room with the same code, and their friends use that code to join the game and audio room. Every user gets stored in a MongoDB collection, and is in itself a document composed of the users game code, socket id (when connection is completed) and current game (the code of the game the user is currently playing).

Users are objects that hold all the information required to work with the game, like their number of chips, their bet amount, wether they are the dealer, etc. This is found in `user.dto.ts`.

Logic related to user management is defined in `user.service.ts`.

## The Audio Room

Every Dogker game is associated with an audio room, so that players can communicate with each other on a 'push-to-talk' fashion.

Every audio room has a unique identifier, which is the same as the code of the game it is part of.

Janus WebRTC and the audiobridge plugin are the technology used to handle the audio communication.
A peer-to-peer connection involving 10 users would require the use of a mesh topology, which could become very resource (network, cpu) intensive on the user side: 

<img src="https://miro.medium.com/max/637/0*EgWw-gpXYh-cmXsW"
	alt="WebRTC peer-to-peer" />

So in Dogker's case, the audiobridge plugin is capable of using an MCU (Multipoint Conferencing Unit) topology, which alleviates the resource use on the user side, with the downside of requiring more computing power on the server:

<img src="https://miro.medium.com/max/700/0*0BoXN4UTFnAc3FAO"
	alt="WebRTC MCU" />

Everything related to Janus and the audiobridge plugin is explained in more detail in the [Dogker server section](server.html).

## The Suggestions 

The Dogker landing page includes a section where anyone is able to send a suggestion with a title and description. The REST API handles this process by storing the suggestion on a collection in the database, and also provides the functionality to list the last 10 suggestions received. All of this is defined in `suggestions.service.ts` and `suggestions.controller.ts`.
