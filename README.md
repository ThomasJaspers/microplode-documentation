# microplode-documentation

------------------------------------------------------------------------------------------------------------

<a name="table-of-contents"></a>
__Table of Contents__

* [Introduction](#introduction)
  * [Purpose of this Project](#purpose-of-this-project)
  * [The Game](#the-game)
* [Development Environment](#development-environment)  
  * [Repositories](#repositories)
  * [HowTo](#howto)
* [Service Interfaces](#service-interfaces)
  * [The Board](#the-board)
  * [Messaging](#messaging)
    * [new-game-event](#new-game-event)
    * [move-event](#move-event)
	* [initialize-event](#initialize-event)
	* [board-changed-event](#board-changed-event)
	* [next-turn-event](#next-turn-event)


<a name="introduction"></a>
# Introduction

<a name="purpose-of-this-project"></a>
## Purpose of this Project

First of all having fun :-). Then learning something about technologies and techniques used when implementing *Microservices*.
But not only the technologies are important, but also comparing the whole "development experience" with building applications
as a monolith. Finally hopefully in the end there will be a working game.

Beside this documention that should always reflect the latest state of the project there is a series of blog posts
that are linked in the following. Those are dealing with the technologies used and experiences made in more detail.

* __Part 1:__ [MicroPlode - A Microservices Experiment](https://blog.codecentric.de/en/2015/11/microplode-a-microservices-experiment/)
* __Part 2:__ [MicroPlode - Implementing the first Microservice](https://blog.codecentric.de/en/2015/11/microplode-implementing-the-first-microservice/)
* __Part 3:__ [MicroPlode - Microservices dialogue](https://blog.codecentric.de/en/2015/12/microplode-microservices-dialogue/)

Working on the blog posts is work-in-progress and the plan is to have still a few more as the project advances.

[top](#table-of-contents)

---------------------------------------------------------------------------------------------------------------------------------------------------------

<a name="the-game"></a>
## The Game

The game idea is leant on a known game principle, namely [Hexplode](https://en.wikipedia.org/wiki/Hexplode).
To simplify things *MicroPlode* is played on a plain 10x10 board as also described in more detail later on.

Players are making moves in turns. On a player's turn that player can occupy an empty field and thus giving it a load of 1.
Or the player can add a load of 1 to a field already occupied by that player and thus increasing that field's load by 1.
If a field has a higher load than the number of surrounding fields that field "explodes" and gives a lot of 1 to each
of those surrounding fields. Each field getting a load this way is afterwards owned by the player causing the explosion.
Any load that field had previously is staying on that field. This way it is possible to create really nice change reactions.

A field in a corner is always having three neighbouring fields. A field at the border is having five and a field in the
midlle of the board is having 8 neighbouring fields.

[top](#table-of-contents)


<a name="development-environment"></a>  
# Development Environment


<a name="repositories"></a>
## Repositories

This is the list of GIT repositories containing the different microservice implementations beloging to this project.

* <https://github.com/ThomasJaspers/microplode-presentationservice>
* <https://github.com/ThomasJaspers/microplode-boardservice>
* <https://github.com/ThomasJaspers/microplode-gameservice>
* <https://github.com/ThomasJaspers/microplode-computerplayerservice>

Following figure shows the interaction of the different microservices.

![alt tag](https://github.com/ThomasJaspers/microplode-documentation/blob/master/events-overview.png)

[top](#table-of-contents)

---------------------------------------------------------------------------------------------------------------------------------------------------------

<a name="howto"></a>
## HowTo

You should get a Terminal (with Tabs!) started and install a RabbitMQ-Server (e.g. via brew install rabbit on Mac OS).

You can start the board-, game- and computerplayerservice all in the same way:

<pre>
mvn clean package

java -jar target/microplode-<NAME>service-<VERSION>.jar
</pre>

To start the presentation service, refer to [its README](https://github.com/ThomasJaspers/microplode-presentationservice/blob/master/README.md)   
TL;DR: 'git clone git@github.com:ThomasJaspers/microplode-presentationservice.git', 'npm install', 'npm start'.

If you want to know, what is going on inside the Messaging-Infrastructure, you could simply use the [RabbitMQ-Management-Plugin](https://www.rabbitmq.com/management.html) 
which should be pre-installed in your installation. Therefore open the following URL in a browser:

<pre>
http://localhost:15672/
</pre>

(Use guest/guest to login.)

[top](#table-of-contents)


<a name="service-interfaces"></a>
# Service Interfaces

<a name="the-board"></a>
## The Board

For the time being the board always consists of 10x10 fields. Each field is identified by a pair of numbers identifying first
the line and then the row of the filed. This is shown in the following:

<pre>
|-------------------------------------------------------------------------------|
|  0/0  |  0/1  |  0/2  |  0/3  |  0/4  |  0/5  |  0/6  |  0/7  |  0/8  |  0/9  |
|-------------------------------------------------------------------------------|
|  1/0  |  1/1  |  1/2  |  1/3  |  1/4  |  1/5  |  1/6  |  1/7  |  1/8  |  1/9  |
|-------------------------------------------------------------------------------|
|  2/0  |  2/1  |  2/2  |  2/3  |  2/4  |  2/5  |  2/6  |  2/7  |  2/8  |  2/9  |
|-------------------------------------------------------------------------------|
...
|-------------------------------------------------------------------------------|
|  9/0  |  9/1  |  9/2  |  9/3  |  9/4  |  9/5  |  9/6  |  9/7  |  9/8  |  9/9  |
|-------------------------------------------------------------------------------|
</pre>

Whenever the board is referred to fields are identified by this pair of number as shown above.

[top](#table-of-contents)

------------------------------------------------------------------------------------------------------------


<a name="messaging"></a>
## Messaging

All messages are sent as JSON strings. The following figure shows an overview of the events to be sent.

![alt tag](https://github.com/ThomasJaspers/microplode-documentation/blob/master/events-overview.png)

The content of the different messages is described in the following.

<a name="new-game-event"></a>
### new-game-event

Following information must be submitted:
+ Number of players (implicitly, currently always 2)
+ Type of each player (computer or human)
+ Unique identifier for each player

Open a browser or CURL [http://localhost:8080/gameservice/start-new-game](http://localhost:8080/gameservice/start-new-game)

__Example:__
<pre>
{
    "event": {
        "type": "new-game",
        "playerList": [
            {
                "id": "1",
                "type": "human"
            }, {
                "id": "2",
                "type": "computer"
            }
        ]
    }
}
</pre>


<a name="move-event"></a>
### move-event

Following information must be submitted:
+ Id of the player that made the move
+ The field the move is made according to the agreed board representation shown above

__Example:__
<pre>
{
    "event": {
        "type": "move",
        "playerId": "1",
        "field-row": 1,
		"field-col": 9
	}
}
</pre>

[top](#table-of-contents)

<a name="initialize-event"></a>
### initialize-event

Following information must be submitted:
+ Basically nothing as this simply resets the board

__Example:__
<pre>
{
    "event": {
        "type": "initialize"
	}
}
</pre>

[top](#table-of-contents)


<a name="board-changed-event"></a>
### board-changed-event

Following information must be submitted:
+ List of all fields and their current state
+ This includes owner and load

__Example:__
<pre>
{
    "event": {
        "type": "board-changed",
        "fieldList": [
            {
                "row": 0,
                "col": 0,
				"load": 2,
				"playerId": "1"
            },
            {
                "row": 0,
                "col": 1,
				"load": 0,
				"playerId": ""
            },
			{
                "row": 0,
                "col": 2,
				"load": 4,
				"playerId": "2"
            },
			...
			{
                "row": 9,
                "col": 9,
				"load": 0,
				"playerId": ""
            }
        ]
    }
}
</pre>

[top](#table-of-contents)

<a name="next-turn-event"></a>
### next-turn-event

Following information must be submitted:
+ The id of the player to make the next move

__Example:__
<pre>
{
    "event": {
        "type": "next-turn",
		"playerId": 2
	}
}
</pre>

[top](#table-of-contents)
