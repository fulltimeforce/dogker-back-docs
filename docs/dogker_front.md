---
layout: default
title: Dogker Front
nav_order: 2
has_children: false
parent: Dogker App
---

# Frontend
{: .fs-8 .no_toc }
The frontend side is mainly built on [Flutter](https://flutter.dev/docs) that help us on building the interfaces, animations, transitions and so on.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Getting started

### Start by cloning the project:

```bash
git clone https://github.com/fulltimeforce/docker-front
```

### Once it is cloned, install the dependencies:

```bash
flutter pub get
```

### Now you can run the app in development mode:

```bash
flutter run --debug
``` 

### To run in production mode, first do:

```bash
flutter run --release
```

### To create a build in production mode, first do:
```bash
flutter build appbundle --release
```

To create a build in production mode to be uploaded to the Google Play Console you'll need the official java keystore (.jks) being used for this project. You can download it here: `https://drive.google.com/file/d/1N5pCK0K4sEDtoY9zl5ACCxzwly2Fn0kp/view?usp=sharing`
Then, on the "android" folder make sure that the password, alias and the keystore path is set correctly.

Finally for starting the build you can use either of this commands:
### To create an AppBundle
```bash
flutter build appbundle --release
```

### To create an Apk
```bash
flutter build apk --release
```

## Project Structure
```
.\
 |--FUENTES\         # Font styles applied in dogker frontend
 |--android\    # Code related to android and its configuration 
 |--assets\           # Icons, sound animations
 |--ios\    # Code related to iOS and its configuration
 |--lib\         # Models, widgets, screens. All Dogker logic
 |--test\         # Test for widgets
```

## Deep Links
Deep Links are the configuration for take a user to the app when the link is clicked. All related code can be found in [`deep_link_provider.dart`](https://github.com/fulltimeforce/docker-mobile/blob/master/lib/providers/deep_link_provider.dart)

**NOTE**: If the user don't have the Dogker app installed, it will redirect to the FTF webpage.

## Janus
A Janus connection needs to be stablished before a player can join a voicechat room. These connection can be found in `initPlatformState()` located in [`plugin_manager.dart`](https://github.com/fulltimeforce/docker-mobile/blob/master/lib/util/plugin_manager.dart). 

Once the Janus connection is stablished, a player can Join a call with the players in the current game. The moment when player press the `Join Call` button, it validates if there's a voicechat created by another user. If no one has created a voicechat (by just clicking `Join Call`), the `_createRoom()` is executed. Otherwise, `_joinRoom()` is applied. All of these logic can be also found in [`plugin_manager.dart`](https://github.com/fulltimeforce/docker-mobile/blob/master/lib/util/plugin_manager.dart).

## Socket connection
The socket connection starts as soon as a player joins a room. This can happen in two instances:

1. At the register page, when a player tries to join a room for the first time.
2. At the splash screen, when a player, who was already in a room, is launching the app and is instantly reconnected to their previous still active room.

The moment when player joins a room, the socket connects to `http://dogker.fulltimeforce.com:5000/`. There are also methods such as:
- `connect(String userId, String code, int avatarId, String username)`: It connects to the socket and executes the `createOrJoinGame` method.
- `disconnect()`: Emits to the backend a disconnection event.
- `cleanGame(String gameCode)`: Emits an event for deleting the room code and create another one `(backend)`.
- `isConnected()`: Validates if socket is turned on.
- `getSocketInstance()`: Returns the socket object which can emit events and set up the event listeners.
- `bet(String gameCode, int amount, String userId)`: Emits an event that sends the amount of chips a player is betting.
- `fold(String gameCode, String userId, {String winnerId)`: Emits an event when a player folds.
- `createOrJoinGame(String userId, String gameCode, String username, int avatarId)`: Emits an event when a player joins or create a room.
- `startGame(String gameCode)`: At the moment when the host press "Start" emits an event.
- `talk(String gameCode, String userId, bool talk, double dB)`: After joining a voice chat, the player is able to turn on the mic whenever he wants to talk. The moment the player press his avatar to talk it emits an event called `talk()` and send it to the `backend`.

All of these methods can be found in [socket_manager.dart](https://github.com/fulltimeforce/docker-mobile/blob/master/lib/models/socket_manager.dart)