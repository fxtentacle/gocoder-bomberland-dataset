# fxtentacle's GoCoder Bomberland Datasets

This dataset contains replays for 
https://www.gocoder.one/bomberland
generated with server version 1068.

See below for an explanation of the different strategies.

### replays-baseline-baseline
This dataset contains 
5,267 replays == 3,727,239 timesteps 
(x6 units)
for the baseline strategy playing against itself.
The games break down as:
* A won: 2474 = 46.9%
* B won: 2511 = 47.6%
* Draw: 282 = 5.5%

### replays-runaway-predator
This dataset contains 
3,832 replays == 2,790,616 timesteps 
(x6 units)
for team A using the runaway strategy
being chased by team B using the predator strategy.
Surprisingly, runaway wins more often:
* A won: 2162 = 56.4%
* B won: 1556 = 40.6%
* Draw: 114 = 3.0%

## How is this useful?

I'm trying to train an AI agent for 
https://www.gocoder.one/bomberland
.
Since I don't resources comparable to OpenAI or DeepMind, 
I will not let the AI discover everything through self-play,
but instead I'll speed up the process by first training
using 
[Exponentially Weighted Imitation Learning for Batched Historical Data](https://papers.nips.cc/paper/2018/hash/4aec1b3435c52abbdf8334ea0e7141e0-Abstract.html)
and then only later introducing self-play.

## How was this data generated?

I let a hand-crafted baseline agent play against itself.
The agent is based on A* path-finding with the following notable score rules:

#### all
* Treat team-mates and all enemies except for the closest one 
  as immovable wood blocks to reduce complexity
* Penalize fields that would covered by enemy bombs if they exploded

#### baseline
* Penalize fields that the closest enemy can reach before us
* Penalize walking into small areas with only one exit, to avoid getting locked in
* Promote running towards enemies, bumping into them and blocking their path
* Boost scores for the center of the map, 
  because it'll be reached by the endgame fire last
* Collect whatever pick-up you can, as long as the score for its path is positive
* When you need to retreat, block the enemy from following by placing a bomb

#### runaway
* If there is no path to the center, clear one
* Run away from enemies and their bombs
* Penalize walking into small areas with only one exit, to avoid getting locked in
* Collect whatever pick-up you can, as long as it's safe to reach

#### predator
* Promote running towards enemies and bumping into them
* Try to restrict the enemy's movement as much as possible
* If an enemy is locked in, place bombs to kill them
* Collect whatever pick-up you can, as long as the score for its path is positive
* When you need to retreat, block the enemy from following by placing a bomb


For simplicity, this agent is completely stateless, 
which means it might start to run towards a pickup 
and then decide against it during the path. 
The result is that the bots sometimes oscillate between two fields.

## Which data is included?

These JSON files include only the replays from the official game server,
so they deliberately DO NOT include any internal state of the agents or the server.
That way, these files should be comparable to the ones 
you will receive after your agent competes in the official tournament.
Also, that makes the files compatible with
https://www.gocoder.one/replays
so that you can easily take a look at how well (or not) 
my bots were playing when I recorded this.

## What's with the filenames?

These files are named as
`WINNER-HP_A-HP_B-TICKS-RANDOM`. 
`WINNER` is A, B or D == draw.
`HP_A` and `HP_B` are each 3 numbers stating the HP 
of each team's units at the end of the game.
`TICKS` is the number of server ticks that were played.

Accordingly, `A-120-000-0619-d1685eb6-2af2-11ec-9bc3-f02f741fb790.json`
is the replay of a game that lasted 619 ticks, where A won 
and the unit HPs at the end of the game were 1, 2, and 0. 

## License

(c) 2021 Hajo Nils Krabbenhöft

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

In short, you may use this however you want, as long as you give proper attribution.


