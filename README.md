# microplode-documentation

------------------------------------------------------------------------------------------------------------

<a name="table-of-contents"></a>
__Table of Contents__

* [Purpose of this Project](#purpose-of-this-project)
* [The Game](#the-game)
* [The Board](#the-board)
* [Messaging](#messaging)
    * [new-game-event](#new-game-event)
    * [move-event](#move-event)

------------------------------------------------------------------------------------------------------------

<a name="purpose-of-this-project"></a>
## Purpose of this Project

First of all having fun :-). Then learning something about technologies and techniques used when implementing *Microservices*.
But not only the technologies are important, but also comparing the whole "development experience" with building applications
as a monolith. Finally hopefully in the end there will be a working game.

Beside this documention that should always reflect the latest state of the project there is a series of blog posts
that are linked in the following. Those are dealing with the technologies used and experiences made in more detail.

[A Microservices Experiment](https://blog.codecentric.de/en/2015/11/microplode-a-microservices-experiment/)

[top](#table-of-contents)

------------------------------------------------------------------------------------------------------------

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

------------------------------------------------------------------------------------------------------------

<a name="the-board"></a>
## The Board

For the time being the board always consists of 10x10 fields. Each field is identified by a unique number as
shown in the following:

<pre>
|-----------------------------------------------------------|
|  0  |  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |  9  |
|-----------------------------------------------------------|
|  10 |  11 |  12 |  13 |  14 |  15 |  16 |  17 |  18 |  19 |
|-----------------------------------------------------------|
|  20 |  21 |  22 |  23 |  24 |  25 |  26 |  27 |  28 |  29 |
|-----------------------------------------------------------|
...
|-----------------------------------------------------------|
|  90 |  91 |  92 |  93 |  94 |  95 |  96 |  97 |  98 |  99 |
|-----------------------------------------------------------|
</pre>

Whenever the board is referred to fields are identified by its number as shown above.

[top](#table-of-contents)

------------------------------------------------------------------------------------------------------------

<a name="the-board"></a>
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
        "playerList": {
            "playerDef": {
                "id": "player1",
                "type": "human"
            }
            "playerDef": {
                "id": "player2",
                "type": "computer"
            }
        }
    }
}
</pre>


<a name="new-game-event"></a>

### move-event

Following information must be submitted:
+ Id of the player that made the move
+ Number of the field the move is made according to the agreed board representation shown above

__Example:__
<pre>
{
    "event": {
        "type": "move",
        "playerId": "player1",
        "field": 45
    }
}
</pre>

[top](#table-of-contents)
