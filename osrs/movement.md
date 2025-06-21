# Movement

Basic Discussion of OSRS Pathing

## Pathfinding

[The OSRS Wiki gives a very good overview](https://oldschool.runescape.wiki/w/Pathfinding), it is one of the primary sources.
It is archived here in case of changes.

Pathfinding uses a Breadth-first search which prioritizes the shortest path.

![Pathfinding_calculations_visualised](https://github.com/user-attachments/assets/25e3eea2-f136-4c5d-8e10-0697af7e19d4)

The order directions are checked in

1. West
2. East
3. South
4. North
5. South-west
6. South-east
7. North-west
8. North-east

The pathfinding algorithm is determined largely by three steps. First, it's determined whether the requested location based on the player's click is a valid candidate for pathing and the requested tiles are selected. Second, a nuanced algorithm is used to determine the target tile and the possible paths. Third, the ultimate destination tile is chosen.
The second step to find the target tile roughly operates according to the following principles:

* Paths must never leave the 128x128 tile area centred at the player's starting location.
* * This is actually a 4x4 center of the 128x128 grid. 
2. The paths with least total tiles travelled are prioritised
3. Paths will prioritise lines in the cardinal directions and generally long, straight lines.

### Map Discussion

Amusingly when this is discussed each group uses a different terminology to talk about the same thing.

| Size | Official Term | Runelite Term | Community Term |
| :-: | :-: | :-: | :-: |
| 8x8 | zone | chunk | zone |
| 64x64 | mapzone | map square | chunk

# References


* Wiki: https://oldschool.runescape.wiki/w/Pathfinding
* GEChallengeM: https://www.youtube.com/watch?v=4xGzq5HcQew
* PurpleGod: https://www.youtube.com/watch?v=DX8lN3r3ALw
* Gnomonkey: https://www.youtube.com/watch?v=CfKUQwVJbi0

Insightful questions:

![image](https://github.com/user-attachments/assets/a35f081d-1874-44da-9893-1c959c709cbc)
![image](https://github.com/user-attachments/assets/06637f4b-3169-4e17-8d5f-3635ae41e89c)
![image](https://github.com/user-attachments/assets/a73b7800-3ee0-4093-ace7-813f7b64a78f)
![image](https://github.com/user-attachments/assets/53b67ccc-1669-4718-8a06-11787ad6a73e)

