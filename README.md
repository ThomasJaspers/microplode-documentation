# microplode-documentation



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

## Messaging

All messages are sent as JSON strings. The following figure shows an overview of the events to be sent.

![alt tag](https://github.com/ThomasJaspers/microplode-documentation/blob/master/events-overview.png)

The content of the different messages is described in the following.

### new-game-event

Following information must be submitted:
+ Number of players (implicitly, currently always 2)
+ Type of each player (computer or human)
+ Unique identifier for each player

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

### move-event

Following information must be submitted:
+ Id of the player that made the move
+ Nbr of the field the move is made to counting from 0 to 99

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