---
layout: post
title:  "Rollback Networking"
date:   2024-01-23 15:11:04 +0100
categories: Blog Networking
---
<h3 style="font-size: 1.4em">Foreword</h3>
Welcome to my first ever blog post! My name is Daan Ruting and currently while writing this a year 2 student at Breda University of Applied Sciences. The programme I'm doing is Creative Media and Game Technologies and this blog post is a result of my selft study project. I hope you'll enjoy reading this as I enjoyed learning more in the domain of network programming.

- <a href="#HEADER0">An introduction to rollback networking</a>
- <a href="#HEADER1">What is rollback networking?</a>
- <a href="#HEADER2">Should your game need rollback networking?</a> 
- <a href="#HEADER3">What are the advantages over other networking algorithms? </a>
- <a href="#HEADER4">How can I make it myself?</a>
- <a href="Conclusion">Conclusion</a>

<h1 style="font-size:1.9em" id="HEADER0">Introduction</h1>
You might've heard of the word rollback netcode float around on the internet, when players are unsatisfied with the online experience having to deal with a lot of latency while playing with or against somebody from overseas or suffering because of a poor connection. Rollback networking or also famously known as rollback netcode among the gaming community is a powerful algorithm that solves the problem of the player experiencing input delay. In this blog post I will be talking about:<br><br>

<h1 style="font-size: 1.9em" id="HEADER1">What is rollback networking?</h1>
Rollback networking is an algorithm that makes it possible for the players input to not suffer through the fluctuating networking latency while staying synchronised with the server or directly to the other player. When one of the players inputs arrive too late on the recipient, the recipient will then rollback to the most recent gamestate that was synchronised. <br><br>What will happen after that is the player who received the input too late compared to the promised time it should have arrived, it will simulate chronologicly through all the inputs it has preformed locally and has received from the other player. This is the core idea of the rollback algorithm where we can always simulate through the inputs received from the client. There is also another algorithm that goes together with this algorithm that predicts the inputs of the other player, but I won't go into that topic in this blog since my knowledge isn't well versed in that domain yet.<br><br>
It is important to understand that the rollback algorithm doesn't rely on how good their connection is between the server or peer. Unlike the delay-based networking which is a more commonly used method of networking among many game genres, rollback networking only has a constant amount of input delay. This is benevicial for the player since the common complaint players will have is that the game isn't responsive or commonly phrase it as 'laggy'. <br><br>
This is especially the case when your game is high action pased where the player performs many actions in a short time span. Games like King of Fighter XV and Samurai Showdown use this algorithm provided by [GGPO](https://www.ggpo.net) an SDK designed to support fighting games being able to play online with rollback networking. <br><br>Alternatively, this algorithm also gets used with games that manages a lot of players on 1 server. For example, [Stumble Guys](https://www.stumbleguys.com) use the networking engine [Quantum](https://www.photonengine.com/quantum), which also contains the rollback algorithm to keep all the players in sync without having to deal with the fluctuating input delay.<br><br>

<h1 style="font-size: 1.9em" id="HEADER2">Should your game have rollback networking?</h1>
There are a couple of key points that you need to concider if your game is worth to apply rollback networking on.
- The Game Genre
- The Project Scope
- The Performance Cost

<h2 style="font-size: 1.4em">The Game Genre</h2>
The genre of the game can tell us a lot what we can expect along the way when making the game. That also applies here where we can estimate how many players are going to be involved in a session and how complex the game is going to be. Common game genre's that have shown to implement this feature are the following:
- Fighting Games
- Battle Royale Games
- Racing Games

<h2 style="font-size: 1.4em">The Project Scope</h2>
Rollback Networking is a algorithm where you have to meticulously plan before hand on how you're going to make your game. You need to concider these <a href="#HEADER4-1">requirements</a> for the algorithm to work with your game. The indie developer Dan Fornace's quoted in a [blog post](https://fornace.medium.com/dan-fornaces-10-tips-for-making-a-fighting-game-e2c982da2396) the following: <i>"It turns out that launching a game without rollback netcode makes adding it in extremely difficult".</i>

<h2 style="font-size: 1.4em">The Performance Cost</h2>
The performance cost of rollback networking is generally not always worth it depending on the game you want to make. Since you need to simulate your game a number of times in 1 rollback within 1 frame, you need to make your gameloop run as preformant as possible. 1 Frame in this context is commonly 1/60th of a second. <br><br> For example in a FPS game where the game needs to preform many raycasts to detect what object it hit can be very costly if it constantly needs to do that. Now if we have for example a lobby of 10 players that are constantly holding the shoot button with a minigun. It will become apparrent that the gameloop might take some time to execute in 1 frame therefore the rollback will fail to catch up with the state of the game the other player is in.

<h1 style="font-size: 1.9em" id="HEADER3">Advantages and Disadvantages</h1>
Now it is important to understand what the advantages are to use this algorithm over the delay-based networking. Delay-based networking is a method where the input delay is decided based on how good your connection is with the server and how fast it thinks the packets will take to arrive to the server and vice versa.

<h2 style="font-size: 1.4em">Advantages</h2>
- Low Latency <br>
The algorithm will make the experience feel more responsive compared to delay-based networking. Since the packets clients will send to the server or player have to be at a low bandwidth it will make the latency also very low.
- Constant Amount Of Input Delay<br>
It's a big plus that players don't have to adapt to a variable of input delay when preforming actions. Of course this number can be change before starting a session judging on the connectivity of both players. This gives them an expectation if this information is presented on how good the connection is. 
- Uninterupted Resynching<br>
With delay-based networking players would usually have to wait for a bit when the session is missing a packet from 1 of the players. With rollback networking the players will not be disrupted in their game and the networking can just keep doing their thing to resynchronise both players when a input has arrived too late.

<h2 style="font-size: 1.4em">Disadvantages</h2>
- Performance Cost <br>
We have to admit as good as this algorithm is for fixing the "lag" issues, it does come with a price in terms of your game's performance. You need to be very carefull what you want to resimulate in your game and how you can later optimise your gameloop.
- Requires Meticulous Planning <br>
In order to implement this algorithm you need be very carefull with how you are going to structure your code and what other libraries you're going to use. 
- Limited Genre Scope <br>
Depending on what time of game you're aiming for, online implementation with rollback will become less realistic to implement due to how the algorithm works.

<h1 style="font-size: 1.9em" id="HEADER4">How can I make rollback networking?</h1>
Now that we understand what this algorithm is all about, lets get started on how you can make this algorithm! Before I go over how you can program the algorithm we first need to go over a couple of things you need to check upon to make sure that programming this algorithm will work.

<h2 style="font-size: 1.4em" id="HEADER4-1">Requirements</h2>
- <a id="DETERMINISM">Determinism</a>
- <a id="FIXEDTIME">Fixed Timestep</a>
- <a id="NETWORKING">Networking</a>
- <a id="STATERESTORE">Gamestate Restoration</a>

Lets first talk about the <b id="DETERMINISM">Determinism</b> of a game. Since the rollback algorithm needs to simulate from a gamestate we need to make very sure that when we give a sequence of inputs, the outcome should always be the exact same result when we run it even a thousand times. Determinism means that we can expect the same output when we give a sequence of inputs. The inputs in this case can be the buttons being pressed from the controller, the amount of tick/frames the program has been running for or the seed that the game uses for randomisation functions. <br><br> There are many factors that could make the game not deterministic for example using the seed generated only from the client based on the time the program has started. If you're programming something where you're not sure what the outcome can be, then your intuition should tell you that you need more determinism. We could name more examples how we can define determinism, but what is important to know is that your program needs to be able to simulate with perfect accuracy.

That brings us to our next topic which is making sure that your game runs on a <b id="FIXEDTIME">Fixed Timestep</b>. Since we need to be able to simulate our game with perfect accuracy we need to determine the rules on what FPS the game should run. This is because the refresh rate and performance of each machine can widely vary from each other and run slower or faster then the other player. By modern standards having a set refresh rate of 60Hz per second should be good to work with, but you can decide on what number you want that to be. 
```cpp
//Pseudo code on how to run your game at a fixed timestep
struct Game;
struct Inputs;
class Program {
    public:
        Program();
        Update(float delta);
        GameUpdate(float delta);
    private:
        Game game;
        float accumalator;
        static constexpr float FIXED_TIMESTEP = 1.f/60.f;
}
Program::Program() {
    accumalator = 0.f;
}
Program::Update(float delta) {
    Inputs _inputs = GetInputs();
    accumalator += delta;
    while(accumalator >= FIXED_TIMESTEP)
    {
        game.Update(FIXED_TIMESTEP, _inputs);
        accumalator -= FIXED_TIMESTEP;
    }
}
Program::GameUpdate(float delta, Inputs inputs) {
    //Runs your game, deterministicly...
}
```
Now on to the next part we finally get to one of the most important aspects of this algorithm, <b id="NETWORKING">Networking</b>. Now depending on how broad your domain knowledge is you can choose to make your own networking feature. You have to be aware that when you're going to use a networking library or make your own networking feature it needs to support UDP. The reason we want to use UDP over TCP is so that our packets gets sent as fast as possible to the recipient players/server.<br><br> For those that prefer to use a networking library I can recommend using GameNetworkingSockets, SLikeNet, Asio or Yojimbo(doesn't support peer 2 peer as of yet). These libraries cover the requirements of what you need for your networking. In my case I use c++ so if you wish to have a networking library supporting the programming language of your choice, you'll have to look into that yourself. <br><br> 

```cpp
//Pseudo code to showcase roughly how networking could be implemented to your game loop.
Program::Update(float delta) {
    UpdateNetwork();
    Inputs _inputs = GetInputs();
    accumalator += delta;
    while(accumalator >= FIXED_TIMESTEP)
    {
        Inputs _combinedInputs = _inputs + _receivedInputs;
        //Send the inputs before updating the game so the networking has more time to send the data.
        SendInputPacket(Inputs);
        game.Update(FIXED_TIMESTEP, _inputs);
        accumalator -= FIXED_TIMESTEP;
    }
}

Program::UpdateNetwork() {
    std::vector<Packet> packets = netlib::GetPackets();
    //loops over received packets and processes them depending what kind of packet it is
    ProcessPackets(packets);
    netlib::ClearReceivedPackets();
}

Program::SendInputPacket(Inputs) {
    //Insert library code to send a message to the server or peer.
}
```

Lastly we'll be going through the part we have to think about <b id=STATERESTORE>State Restoration</b>. State restoration to put it briefly is how we can save and restore our gamestate. This is a core feature we need for this algorithm since we need to be able to rewind back to a previous or older gamestate when we receive a packet of inputs too late that was designated to be executed a couple of frames later. <br><br> What is important to note is that we don't have actually copy the exact state of our whole programme since we're not going to need to restore any render buffers or particles for example. You only need to be able to restore the states that affect directly to the gameplay such as the character data, gamemode data and prop data. Here you will have to be carefull that data like audioplayers don't revert to an older state since that will cause audio artifacts in your game when preforming a rollback.

<h1 style="font-size: 1.9em">Conclusion</h1>
Now that we have gone through the important aspects before programming the rollback algorithm that's where I'm going to conclude this blogpost for the time being, since I haven't gotten to the part of implementing rollback myself yet. I would've loved to show more details on how to implement it but with my current knowledge, I'm not confident yet to make uneducated showcase with code examples for you, the reader to learn from. I will write a follow up blog post in the near future where I'm going to go into implementing the rollback algorithm with an example game. I hope you've enjoyed reading this blog post as I did writing it and perhaps sparked an interest in network programming.

<h2 style="font-size: 1.4em">Recommended resources</h2>
- <a href="https://www.photonengine.com/quantum">Photon Quantum</a>
- <a href="https://github.com/pond3r/ggpo">GGPO</a>
- <a href="https://fornace.medium.com/rivals-on-switch-the-biggest-challenge-of-my-career-f2ebaf7280f3">Article of Dan Fornace's journey of implementing rollback networking</a>
- <a href="https://youtu.be/7jb0FOcImdg?si=t3O_9JAxhpSYQnjt">GDC talk from Mortal Kombat and Injustice 2 developer about optimising the game for rollback</a>
- <a href="https://en.wikipedia.org/wiki/Netcode">A brief wikipedia page explaining how rollback networking works</a>