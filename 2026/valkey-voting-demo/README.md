# Valkey Voting Demo

simulate creating and managing votes for a multi-round voting game like Survivor
managed with Valkey

## Prerequisites
- install [Valkey](https://valkey.io/topics/installation/)
- run Valkey server `valkey server`
- run Valkey cli `valkey-cli`

## What the Script Does

- Create a "Room"
- Create several "Players"
- Simulate several rounds of voting

## Examples

```sh
valkey-cli < single_room.txt

valkey-cli # execute commands in the text block below for player stats

# when you're done
echo "FLUSHALL" | valkey-cli
```

```txt
HGETALL room:ABCD
HGETALL ABCD:1
LRANGE ABCD:1:votes 0 -1
```

## Value Encoding

Note that while you can use the list indices to encode the values, we could also just encode them directly in the list elements to get a concrete encoding (what if a player gets blocked from voting for a round). Then we can just keep track the offset in the app and use the modulo of that to get the vote. 

This assumes a max player count of 9 though, so...maybe there's a better approach to encode the information. They're strings anyways. 

## TODO
- Make a demo with [Streams](https://valkey.io/topics/streams-intro/). 
- Make a multi-room demo