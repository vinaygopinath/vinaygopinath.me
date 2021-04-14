---
title: "Jodi Sudoku"
description: "Open-source, privacy-friendly multiplayer WebRTC Sudoku game"
date: 2021-04-13T23:02:22+03:00
categories: ["tech"]
tags: ["React", "Rails"]
---

Happy to share that I'm working on [Jodi Sudoku](https://jodisudoku.app), an open-source ([AGPLv3](https://choosealicense.com/licenses/agpl-3.0/)), Sudoku progressive web app with the goal of implementing multiplayer support using WebRTC!

I've been following the adoption of [WebRTC](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API) (now supported on [all major platforms](http://iswebrtcreadyyet.com/legacy.html) except Safari, [infamous for dragging its feet](https://onezero.medium.com/apple-is-trying-to-kill-web-technology-a274237c174d)) and the decentralized web for years, but haven't had the opportunity to work with it, so I decided to use it in a hobby project.

## Frontend
The [frontend code](https://github.com/vinaygopinath/jodi-sudoku) uses the standard React + Typescript setup, with React Router and Redux. So far, the web app features
* Starting a new game with a level of difficulty of your choice
* Keyboard and mouse/touch input
  * Changing the mode of entering values into cells - Choose a digit first, and then click a cell to enter the digit, or vice versa
* Responsive cross-platform layout, tested on Firefox and Chrome (desktop and mobile)
* Undo and redo using redux store history

I'm a big fan of internationalization and [supporting regional languages](https://www.vinaygopinath.me/2019/06/hey-big-tech-support-regional-languages/), so the basic one-player implementation currently supports Kannada, English and Polish using react-i18n.

## Backend

The [backend](https://github.com/vinaygopinath/jodi-sudoku-api) is more esoteric - I'm interested in using Rails + WebSockets. While I primarily wear the Android hat at [work](https://maishameds.org/), our backend is built with Rails (a framework I had not worked with before) and I'm interested in improving my Rails fu through this project and contributing more than just the occasional pull request. Rails has a thin wrapper over WebSockets called [Action Cable](https://guides.rubyonrails.org/action_cable_overview.html), and I'm curious to see how it fares compared to Node ( + socket.io), which seems to be the internet's go-to recommendation for scalable WebSockets. WebSockets is a great way to achieve peer discovery (your browser tab discovering peers and finding a way to [establish a direct connection despite NAT traversal](https://www.html5rocks.com/en/tutorials/webrtc/infrastructure/)), required for WebRTC. If there's a lot of traffic and Action Cable/Rails struggles under the load, I'm aware I might need to swap out Action Cable for Node someday, but I think the Rails experimentation will be worth it. Besides, Node feels like home turf and it'll be fun to try something different :)

## Multiplayer

What's with the idea of multiplayer Sudoku, you ask? Is there any interest in multiplayer Sudoku? I'm not sure - I found a few multiplayer games on different app stores, but I don't think there's a big community. This project is honestly just an excuse to find a way to play Sudoku with my mum - Playing Sudoku together is a ritual whenever I'm in Bangalore. With mugs of basil and ginger tea after dinner, and armed with pens at the kitchen table, we pair up (Jodi (ಜೋಡಿ) is the Kannada word for "pair") on solving the Sudoku puzzle in the daily newspaper (remember those?), trying to get as many digits as possible but also explaining to each other how we "unlocked" the right digits. I miss that, and I see no reason why that should stop when I'm not in Bangalore :) I'd be happy if others find it useful as well.

The idea is to implement URL-based discovery of peers. Like [Jitsi](https://meet.jit.si/some-room-name) calls, users who open the same link will be able to connect to the server using WebSockets, receive information from the server on the users in the "room", and then establish browser-to-browser WebRTC peer connections, making the server connection theoretically unnecessary after that point.

I'm thinking of building different multiplayer modes
* Cooperative: all players in a room collaboratively solve the same puzzle together
* Challenge (same puzzle): all players in a room _individually_ solve the same puzzle, only aware of the number of empty/filled cells of other players.
* Challenge (puzzle of same difficulty): All players in a room solve different puzzles of the same difficulty level, only aware of the number of empty/filled cells of other players. Could be fun to also add a "Peek player's board" feature
* Time challenge? Turn-based games? Some dastardly variant of Sudoku?

### Q: Privacy and fault tolerance

I've encountered a lot of flaky internet connections and I'd like Jodi Sudoku to be tolerant of and handle edge cases related to flaky connections, users having to refresh their tabs, abrupt drop-offs etc. Given that exploring WebRTC and peer-to-peer systems is one of the goals, it's imperative that the server know as little as possible about users, rooms or the type and state of the game in a room.

If the server knows nothing about the state of the game in a room, who is the source of truth in case of failure? Should all players in a room store the game state locally (in the browser local/session storage), and send the most recent state (via WebRTC) to any player who re/joins a room? What happens if two players disagree on the most recent state of the game? Should rooms have a leader who is responsible for storing the game state and sharing it with users who enter the room? What happens if the leader's connection goes down? I have struggled to square the two seemingly contradictory goals of privacy/decentralization, and maintaining a source of truth for fault recovery, and decided to move forward with making the server aware of users, rooms, and game states. Please reach out if you have any suggestions that would keep the server out of the loop after peer discovery.

My current approach is:

#### Peer discovery
1. Abbi enters a room (a room is identified by the link, i.e, judisudoku.app/some-room)
2. Abbi establishes a WebSocket connection with the server.
3. Abbi receives the list of users in the room via WebSocket. The list does not have anyone except Abbi. The WebSocket connection with the server is kept open.
4. Ilana enters the room, connects to the server and receives the list of users. The WebSocket connection with the server is kept open.
5. Abbi and Ilana use a STUN server, bypass their NAT, and establish a peer-to-peer connection.

#### Gameplay
6. Abbi and Ilana begin a cooperative game, notifying each other via WebRTC and notifying the server via WebSocket.
7. Abbi and Ilana send messages directly to each other via WebRTC when they enter values into cells. They would also need to passively notify the server of the state changes via WebSocket
8. The server passively stores the game state, doing nothing with it.

#### New player/rejoin
9. Jaime joins the game late, and is announced to the other users by the server (the updated list of users is shared) via WebSocket
10. The server shares the latest state of the game to Jaime via WebSocket
11. Once connected to the other users via WebRTC p2p connections, Jaime follows step 7 to participate in the game.

Using the server as the single source of truth allows users who join a game midway or experience connection issues and rejoin the game to continue the (collaborative) game from the latest state (if other users have made changes). Other multiplayer modes may need to handle failure/rejoin scenarios differently.

## Next steps
I realise I've defined a lot of lofty technical ambitions for a simple Sudoku webapp, and I'd like to take this one step at a time. I'm planning to make small releases to keep up my motivation and get feedback from my target demographic of one - When it comes to hobby projects, I tend to be very excited during the design phase and in the early days of implementation, and lose interest and move on to the next shiny puzzle that needs solving when the problem is more or less "solved" in my head :)

The next release will have peer discovery and cooperative game mode.