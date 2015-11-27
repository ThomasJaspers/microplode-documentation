# microplode-documentation



## Messaging

All messages are sent as JSON strings. The content of the different messages is described in the following.

![alt tag](https://github.com/ThomasJaspers/microplode-documentation/blob/master/events-overview.png)

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