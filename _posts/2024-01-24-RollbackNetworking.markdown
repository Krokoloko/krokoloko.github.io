---
layout: post
title:  "Rollback Networking"
date:   2024-01-23 15:11:04 +0100
categories: Blog Networking
---

You might've heard of the word rollback netcode float around on the internet, when players are unsatisfied with the online experience having to deal with a lot of latency while playing with or against somebody from overseas or suffering because of a poor connection. Rollback networking or also famously known as rollback netcode among the gaming community is a powerful algorithm that solves the problem of the player experiencing input delay. In this blog post I will be talking about:<br><br>

- <a href="#HEADER1">What is rollback networking?</a>
- <a href="#HEADER2">Should your game need rollback networking?</a> 
- <a href="#HEADER3">What are the advantages over other networking algorithms? </a>
- <a href="#HEADER4">How can I make it myself?</a>

<h1 style="font-size: 1.9em" id="#HEADER1">What is rollback networking?</h1>
Rollback networking is an algorithm that makes it possible for the players input to not suffer through the fluctuating networking latency while staying synchronised with the server or directly to the other player. When one of the players inputs arrive too late on the recipient, the recipient will then rollback to the most recent gamestate that was synchronised. <br><br>What will happen after that is the player who received the input too late compared to the promised time it should have arrived, it will simulate chronologicly through all the inputs it has preformed locally and has received from the other player. This is the core idea of the rollback algorithm where we can always simulate through the inputs received from the client. There is also another algorithm that goes together with this algorithm that predicts the inputs of the other player, but I won't go into that topic in this blog since my knowledge isn't well versed in that domain yet.<br><br>
It is important to understand that the rollback algorithm doesn't rely on how good their connection is between the server or peer. Unlike the delay based networking which is a more commonly used method of networking among many game genres, rollback networking only has a constant amount of input delay. This is benevicial for the player since the common complaint players will have is that the game isn't responsive or commonly phrase it as 'laggy'. <br><br>
This is especially the case when your game is high action pased where the player preforms many actions in a short time span. Games like King of Fighter XV and Samurai Showdown use this algorithm provided by [GGPO](https://www.ggpo.net), an SDK designed to support fighting games being able to play online with rollback networking. <br><br>Alternatively, this algorithm also gets used with games that manages a lot of players on 1 server. For example [Stumble Guys](https://www.stumbleguys.com) use the networking engine [Quantum](https://www.photonengine.com/quantum) which also contains the rollback algorithm to keep all the players in sync without having to deal with the fluctuating input delay.<br><br>


<h1 style="font-size: 1.9em" id="HEADER2">Should your game have rollback networking?</h1>
There are a couple of key points that you need to concider if your game is worth to apply rollback networking on.
- The Game Genre
- The Project Scope
- The performance cost

<h2 style="font-size: 1.55em">The Game Genre</h2>
The genre of the game can tell us a lot what we can expect along the way when making the game. That also applies in this case where we can estimate how many players are going to be involved in a session and how complex the game is going to be. Common game genre's that have shown to implement this feature are the following:
- Fighting Games
- Battle Royale Games
- Racing Games

<h2 style="font-size: 1.55em">The Project Scope</h2>
Rollback Networking is a algorithm where you have to meticlously plan before hand on how you're going to make your game. You need to concider these <a href="#HEADER4-1">requirements</a> for the algorithm to work with your game. The indie developer Dan Fornace's quoted in a [blog post](https://fornace.medium.com/dan-fornaces-10-tips-for-making-a-fighting-game-e2c982da2396) the following: <i>"It turns out that launching a game without rollback netcode makes adding it back in extremely difficult".</i>

<h2 style="font-size: 1.55em">The Performance Cost</h2>
The performance cost of rollback networking is generally not always worth it depending on the game you want to make. Since you need to simulate your game a number of times in 1 rollback within 1 frame, you need to make your gameloop run as preformant as possible. 1 Frame in this context is commonly 1/60th of a second. <br><br> For example in a FPS game where the game needs to preform many raycasts to detect what object it hit can be very costly if it constantly needs to do that. Now if we have for example a lobby of 10 players that are constantly holding the shoot button with a minigun. It will become apparrent that the gameloop might take some time to execute in 1 frame therefore the rollback will fail to catch up with the state of the game the other player is in.

<h1 style="font-size: 1.9em" id="HEADER3">Advantages and Disadvantages</h1>


<h1 style="font-size: 1.9em" id="HEADER4">How can I make rollback networking?</h1>

<h2 style="font-size: 1.55em" id="HEADER4-1">Requirements</h2>


<h1 style="font-size: 1.9em">Conclusion</h1>