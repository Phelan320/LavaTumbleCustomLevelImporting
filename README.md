# Lava Tumble Custom Level Importing
[Lava Tumble](https://www.roblox.com/games/59273753/Lava-Tumble) is a game on ROBLOX. This repository provides 
documentation and samples that describe how to create and import your own custom levels.

## Prerequisites
Importing custom levels is permitted exclusively within Lava Tumble 
[VIP Servers](http://wiki.roblox.com/index.php?title=VIP_server) so it is necessary to purchase a VIP Server 
subscription. The reason for this requirement is that it is possible to generate custom levels that simply do not work 
or are otherwise detrimental to the Lava Tumble experience. Restricting custom levels to VIP Servers ensures that the 
damage caused by malignant or incorrect levels is contained. Don't worry. If you accidentally upload a malformed level, 
simply restart your VIP server to clear all custom levels.

## Importing
The remainder of this document will describe how to create and format your custom level. 

### Using pastebin.com
Once your level has been 
created, make it available online by creating a new paste at [pastebin.com](https://www.pastebin.com). When you have 
saved your level there, copy the 8 character identifier from the pastebin url for your level. Enter your Lava Tumble VIP
Server and enter the following in chat:
`/load xxxxxxxx` where `xxxxxxxx` is the 8 character identifier you copied from the url. If your level is properly 
formatted and loaded by the system, you'll hear a chime and see a message that your level has been accepted. Your level
will then appear on the `Custom` tab of the level selection UI. This will generally be the last of the tabs.

### Elsewhere on the web (including github.com)
You can simply enter `/load <YOUR URL HERE>` into chat to load your level from anywhere on the web including github. 
Note that the url must return text, not html. 
Use this command to load `Mirror's Edge` from the samples in this repository:
```
/load https://raw.githubusercontent.com/AxeOfMen/LavaTumbleCustomLevelImporting/master/samples/42%20-%20Mirror's%20Edge.json
```

## The Single Player Levels
The single player levels have been included in this repository to serve as examples of how to format your custom
levels. They may also serve as a starting point for your custom level. You could take the _Tangent_ level, for instance, 
and modify its properties to increase the time the player must stay alive. 

## An Example
As an introduction to the level format, we'll use the level _Stayin' Alive_ as an example. Here is the level:
```
{
    "name": "Stayin' Alive",
    "instructions": "Stay alive for 15 seconds",
    "entry": [6, 1, 6],
    "awaitOutcome": "keepAlive",
    "gameModes": ["randomBlocksDrop"],
    "gameplayOptions": {
        "requiredDuration": 15,
        "blockFallInterval": 0.5
    },
    "map": [
        [
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
            [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
        ]
    ]
}
```
We'll now examine the properties of this level and what they do. 
### name (required)
Each level has a name. It is a string that identifies the level to the user. It is displayed in the level selection 
window as well as the `Now Playing` UI.  It can be anything you want, but keep in mind that long names may overflow 
their UI containers and look silly.

### instructions (required)
A short description of what the player must do to pass the level. This is displayed to the player shortly before the 
level begins and in the `Now Playing` UI.

### entry (required)
This describes the location on the level that the player will be placed initially. The 1st number (6) represents the
column in the `map`. The 2nd number represents the layer (our example has only one layer, so the only option is 1 in this 
case). The 3rd number (6) represents the row in the `map`. Using the `map` property from our example, we count six 
columns over on the first row. This is the column the player will start on. Some maps have multiple layers (like 
Deadfall), up to 6. But our example has only one layer so the entry point specifies layer 1 as the entry layer. Now 
count down six rows on the map and you will come to the very center of the map. The entry point `[6, 1, 6]` is the 
center of the map on the first layer. See the documentation of the `map` property below for more information.

### awaitOutcome (required)
This describes the win conditions for the level. There are a handful of predefined win conditions that you can choose 
from and they are documented later in this document. You must specify a single `awaitOutcome` value for your custom 
level. The value in our example is `keepAlive` which describes a win condition by which the player must remain alive 
for a specified amount of time.

### gameModes
This describes a set of behaviors for your level. Like awaitOutcome, there is a predefined set of these available to 
you. For `gameModes` however, you can combine multiple values or use none at all. In our example, there is a single 
`gameMode` value specified, `randomBlocksDrop`. This game mode causes random blocks to fall from the map at a specified 
interval.
 
### gameplayOptions
Some values for `awaitOutcome` and `gameModes` permit customization. When they do, you can include `gameplayOptions` to
specify the values you want to customize. In our example, we are using the `awaitOutcome` value of `keepAlive`. This 
`awaitOutcome` expects a `gameplayOption` called `requiredDuration` which specifies how long the player must remain 
alive to pass the level. We are also using the `gameModes` value of `randomBlocksDrop` which expects a `gameplayOption` 
of `blockFallInterval` which configures how many seconds will elapse between blocks falling. The value specified is 
`0.5` which means that half a second will elapse between each block falling.
 
 ### map (required)
 The `map` is a 3 dimensional array describing the physical presence of blocks in your level. The maximum dimensions of 
 the `map` array is `11 x 6 x 11`. 11 on the x and z axes and 6 on the y axis. Wherever a `1` appears in the map, a 
 block will be present. A `0` represents the absence of a block - no block will be placed where there is a zero in the 
 map. To visualize how the `map` property is used, imagine that you are looking from the top down onto your level. Where 
 you see a `1` is where a block will be on your map. In our example, the entire top layer is full of blocks.
 As another example, here is the map for Diagonals:
 ```
 "map": [
     [
         [0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
         [1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0],
         [0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
         [0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
         [0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
         [1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0],
         [0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
         [0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
         [0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0],
         [1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 0],
         [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
     ],
     [
         [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
         [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0],
         [0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0],
         [0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0],
         [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0],
         [0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0],
         [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0],
         [0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0],
         [0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0],
         [0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0],
         [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
     ]
 ]
 ```
 In Diagonals, you can see that there are two layers. The first layer is the top layer of the map. The second layer is 
 the second layer of your custom level and so on, up to six layers.
 
 ### blockPropertyOverrides
 Our example does not use this property, but you will see this property in some of the other examples in this 
 repository. Most commonly, you will see this property used in levels where the objective is to touch all the green 
 blocks. The property is used to override the default properties of a block, like changing the color of the block. Here
 is an example of its use from the level Punchline:
 ```
     "blockPropertyOverrides": [{
             "blockPositions": [[6, 1, 11]],
             "overrides": {
                 "BrickColor": "Bright green"
             }
         }
     ]
```
This usage of `blockPropertyOverrides` tells the level engine to change the color of the block located at `[6, 1, 11]` 
to Bright green. You can change the properties of blocks that are represented by native types (boolean, number, string)
and you can also set the BrickColor using any of the 
[BrickColor names](http://wiki.roblox.com/index.php?title=BrickColor_codes). Some colors have special meaning in some 
`gameModes` like the multi-touch blocks found in many of the levels in the Preview pack and the gray blocks in the level 
_...Now you Don't_. Those will be decribed later in the documentation.

## Reference
In this section you will find documentation on the permitted values for the `awaitOutcome` and `gameModes` properties 
along with the `gameplayOptions` that apply to each value.

### awaitOutcome
- allBlocksTouched 
  - All blocks must be touched. 
  - Example levels: _Between the Lines_, _The Complex_
  - There are no `gameplayOptions` associated with this `awaitOutcome` value
- allSpecifiedBlocksTouched
  - The specified blocks must be touched. 
  - Example levels: _Punchline_, _First Aid_ 
  - `gameplayOptions`
    - `blocksToRemove`: Blocks with the specified value for the specified property represent the blocks that must be touched. 
      - `property`: The name of the block property 
      - `value`: The value of the block property
  - `blockPropertyOverrides` Give specific blocks the properties that mark them as needing to be touched 
    - `blockPositions`: An array of coordinates specifying which blocks will have properties overridden.
    - `overrides`: An object containing name/value pairs where the name is the block property name and the value is the value to be assigned to that block property.
- keepAlive
  - The player must stay alive for a specified duration. 
  - Example levels: _Stayin' Alive_, _Still Alive_, _Tangent_
  - `gameplayOptions`
    - `requiredDuration`: The number of seconds the player must stay alive.
- starredIsLastTouched
  - The player must touch the starred block last. 
  - Example level: _Final Lap_ (in the preview levels)
  - `gameplayOptions`
    - `lastBlock`: An array of x,y,z coordinates describing the position of the last block, the block that will have a 
    star on it.

### gameModes

- `randomBlocksDrop`
  - Randomly selected blocks fall from the map at a specified interval
  - Example levels: _Stayin' Alive_, _Still Alive_
  - `gameplayOptions`
    - `blockFallInterval`: A number representing the number of seconds between blocks falling
- `mysteryPath`
  - Blocks are generated over time
  - Example levels: _Tangent_, _The Runaround_
  - `gameplayOptions`
    - `head`: An array of x,y,z coordinates describing the position of an existing block in your map which will serve as the location from which the blocks will begin to appear
    - `blockInterval`: The number of seconds to wait between block generations
- `portals`
  - Touching a blue block teleports the player to a corresponding orange block
  - Example levels: _Separation Anxiety_, _Here and There_
  - `gameplayOptions`
    - `portalSets`: An array of objects, each with a `blue` and an  `orange` property. In both cases, the value of `blue` and `orange` is an array of x,y,z coordinates describing the position of the block
  - `blockPropertyOverrides` If you want the blocks to be colored before the player starts, add overrides to color them here. Otherwise, the blocks will be colored after the countdown (this can result in some intentional apprehension for the player) 
    - `blockPositions`: An array of coordinates specifying which blocks will have properties overridden.
    - `overrides`: An object containing name/value pairs where the name is the block property name and the value is the value to be assigned to that block property.
        - The colors used are `Bright bluish green` and `Deep orange`
- `horizontalMirrorMode`
  - Players must wend a path through explosive blocks that are identified by means of a mirrored level on the opposite side of the map
  - Example levels: _Mirror's Edge_, _Reflections_
  - `blockPropertyOverrides`
    - `blockPositions`: An array of coordinates specifying which blocks will have properties overridden.
    - `overrides`: An object containing name/value pairs where the name is the block property name and the value is the value to be assigned to that block property.
      - Blocks that are colored `Bright red` in the mirrored map cause the player to die in a horrible explosion. 
- `hiddenBlocks`
  - Specified blocks become transparent and difficult to see
  - Example level: _...Now You Don't_
  - `blockPropertyOverrides`
    - `blockPositions`: An array of coordinates specifying which blocks will have properties overridden.
    - `overrides`: An object containing name/value pairs where the name is the block property name and the value is the value to be assigned to that block property.
      - Blocks that are colored `Mid gray` become transparent 
- `multiStepBlocks`
  - Blocks must be touched more than once (but not in succession - they turn red and explosive until a different block is touched) before they fall
  - Example level: _Final Lap_
  - `blockPropertyOverrides` If you want the blocks to be colored before the player starts, add overrides to color them here. Otherwise, the blocks will be colored after the countdown (this can result in some intentional apprehension for the player) 
    - `blockPositions`: An array of coordinates specifying which blocks will have properties overridden.
    - `overrides`: An object containing name/value pairs where the name is the block property name and the value is the value to be assigned to that block property.
        - The following colors describe how many times the block must be touched before it falls:

            	`Wheat` - 1 jump
            	`Brick yellow` - 2 jumps
            	`Cashmere` - 3 jumps
            	`Burlap` - 4 jumps
            	`Cork` - 5 jumps

