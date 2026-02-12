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

# run the shell and execute the below commands
valkey-cli

# when you're done
echo "FLUSHALL" | valkey-cli
```

When provided

```txt
HGETALL room:ABCD
HGETALL ABCD:1
LRANGE ABCD:1:votes 0 -1
```

You get this as output, showing how we can store metadata about the game state and track votes. We can then query this once a game is over to pull game statistics, etc.

```txt
127.0.0.1:6379> HGETALL room:ABCD
1) "owner"
2) "Carson"
3) "max_players"
4) "8"
127.0.0.1:6379> HGETALL ABCD:1
1) "name"
2) "Carson"
3) "role"
4) "Innocent"
127.0.0.1:6379> LRANGE ABCD:1:votes 0 -1
1) "2"
2) "2"
3) "5"
4) "2"
```

## Value Encoding

Note that while you can use the list indices to encode the values, we could also just encode them directly in the list elements to get a concrete encoding.

If a player were to be kept from voting in a round, we could still submit a default value (-1) in their place to show that they were still alive, but that their vote for the round won't count.

Here's a few ways we can encode values to indicate the round and the vote. (indices as rounds, encoding round as part of the value, short prefixes, JSON object)

```txt
2
12
r1v2
{"r": 1, "v": 2}
```

Which would then be read in the service and parsed out appropriately.

## TODO
- Make a demo with [Streams](https://valkey.io/topics/streams-intro/). 
- Make a multi-room demo