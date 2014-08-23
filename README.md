### Terrella World Description Format

---

This document explains the how to create and use world description files for Terrella. Terrella is a game created for iOS. Additional details can be found on [the Terrella web site](http://www.quietspark.com/terrella/). Some knowledge of gameplay is assumed.

Terrella world description files are simply JSON files with a "fwd" file extension and so referred to as "fwd" files. The root object is a dictionary that must contain a "levels" element. "levels" is an array of level ojects which are documented below. 

Keep in mind that there is limited error checking for fwd files at the moment, and setting ridiculous values is likely to have ridiculous side effects.

##### Level

---

* `arena` (bool): *Optional. Defaults to false.*
  * Indicates a multiplayer level. Should not be included in external fwd files unless set to false.
* `bonus` (number): *Optional. Defaults to 0.*
  * Starting value for this level's bonus.
* `desc` (string): *Optional. Defaults to "".*
  * The description for this level which appears in the flavor text display the first time a level is played.
* `hidebo` (bool): *Optional. Defaults to false.*
  * Indicates whether or not the blue and orange buttons should be hidden for this level.
* `setID` (string): *Required.*
  * The setID is used to group levels together in the level picker. This is also used as the set's display name for external fwd files.
* `worldID` (string): *Required.*
  * A unique identifier to this level.
* `worldName` (string): *Optional (but strongly encouraged!). Defaults to "".*
  * The display name for this level. Shown in the the level picker and elsewhere.
* `worldVersion` (number): *Required*
  * The version of this level. Changing a level in a fwd file will not necessarily update the world in game unless the `worldVersion` is different than the currently loaded version. 
* `objects` (array of object): *Required*
  * Described below
  
##### Object

---

An object's properties are dependent on the value of the `fc` propery which is an enumeration indicating the type of in-game object. Valid `fc` values are:

* `0`: *Orb*
* `1`: *Gate*
* `2`: *Goal*
* `3`: *Black Hole*
* `4`: *Terrella*
* `5`: *Round Wall*
* `6`: *Switch*
* `7`: *Wall*
* `8`: *Checkpoint*

Dimensions are in "meters" which span 20 points on screen at the normal zoom level. While not enforced, size and radius dimensions should be a multiple of 0.05 meters. Some typical values are:

* Smaller wall dimension: 0.4 meters
* Terrella's radius: 0.25 meters (or 10 points across at normal zoom)
* Typical goal radius: 1.6 meters

There is no expected origin or boundary for a level. The game will determine the bounding box for each level and transform all coordinates as needed. It's also helpful to know that the gravity of an orb is limited to a maximum distance of 24 meters unlike real gravity.

Every object must also contain a point `c` which defines the object's center. Points, energy, and somtimes sizes are encountered frequently so let me define those before moving on to the specific types of game objects. 

##### Point

---

* `x` (number): *Required. x-coordinate*
* `y` (number): *Required. y-coordinate*


##### Size

---

* `h` (number): *Required. Height*
* `w` (number): *Required. Width*

##### Energy (aka color, e)

---

Energy / color is an enumeration with one of the following values:

* `1`: Red
* `2`: Green
* `3`: Blue
* `4`: Orange

---

#### Object Types

The following section describes the properties allowed for each object type based on the value of `fc`.

##### Orb

---

* `fc` (number): *Must be 0 to indicate an orb*
* `c` (point): *Required.*
  * Center of orb.
* `br` (number): *Optional. Defaults to 0.0.*
  * Birthrate, or how often a clone of this orb is created. Expressed in seconds.
* `e` (energy): *Required*
  * Energy, aka color.
* `fx` (bool): *Optional. Defaults to true.*
  * Indicates orb is fixed if true.
* `r` (number): *Optional. Defaults to 0.8*
  * Radius of orb.
* `rs` (number): *Optional. Defaults to 6.0.*
  * Respawn interval, or how long after this orb is destroyed should it respawn. Set to 0.0 to never respawn. Expressed in seconds.
* `v` (point): *Optional. Defaults to (0.0, 0.0)*
  * Initial velocity of orb in m/s.

##### Gate

---

* `fc` (number): *Must be 1 to indicate a gate*
* `c` (point): *Required.*
  * Center of gate.
* `e` (energy): *Required*
  * Energy, aka color.
* `r` (number): *Optional. Defaults to 0.0*
  * Rotation of gate. Expressed in radians.
* `s` (size): *Optional. Defaults to (0.0, 0.0)*
  * Size of gate. Typically the smaller dimension is 0.4.
  
##### Goal

---

* `fc` (number): *Must be 2 to indicate a goal*
* `c` (point): *Required.*
  * Center of goal.
* `ptValue` (number): *Optional. Defaults to 1000*
  * The amount of points this goal is worth.
* `r` (number): *Optional. Defaults to 0.8*
  * Radius of goal.
 
##### Black Hole

---

* `fc` (number): *Must be 3 to indicate a black hole*
* `c` (point): *Required.*
  * Center of black hole.
* `r` (number): *Optional. Defaults to 0.8*
  * Radius of black hole.
 
##### Terrella (player)

---

* `fc` (number): *Must be 4 to indicate a player*
* `c` (point): *Required.*
  * Starting point of player.
* `idx` (number): *Optional. Default to 0*
  * Player index. Only needed for multiplayer levels.
  
##### Round Wall

---

* `fc` (number): *Must be 5 to indicate a round wall*
* `c` (point): *Required.*
  * Initial center point of wall.
* `fx` (bool): *Optional. Defaults to true.*
  * Indicates wall is fixed if true.
* `on` (bool): *Optional. Defaults to false*
  * Indicates wall is energized, aka deadly.
* `r` (number): *Optional. Defaults to 0.8*
  * Radius of round wall.
  
##### Switch

---

* `fc` (number): *Must be 6 to indicate a switch*
* `c` (point): *Required.*
  * Center of switch.
* `e` (energy): *Required*
  * Energy, aka color.
* `r` (number): *Optional. Defaults to 0.0*
  * Rotation of switch. Expressed in radians.
* `s` (size): *Optional. Defaults to (0.0, 0.0)*
  * Size of switch. Switched should have the same height and width.
  
##### Wall

---
Although walls appear pill shaped on screen, the physics engine models them as rectangles.

* `fc` (number): *Must be 7 to indicate a wall*
* `c` (point): *Required.*
  * Initial center point of wall.
* `fx` (bool): *Optional. Defaults to true.*
  * Indicates wall is fixed if true.
* `on` (bool): *Optional. Defaults to false*
  * Indicates wall is energized, aka deadly.
* `pin` (bool): *Optional. Defaults to false*
  * Indicates wall is pinned and my rotate about its center. Only applies to walls where `fx` is false.
* `r` (number): *Optional. Defaults to 0.0*
  * Rotation of wall. Expressed in radians.
* `s` (size): *Optional. Defaults to (0.0, 0.0)*
  * Size of wall. Typically the smaller dimension is 0.4.
  
##### Checkpoint

---
A checkpoint may have any size, but only a 34 x 34 image is visible onscreen. The center of the image is placed at the center of the checkpoint offset by `soff`.

* `fc` (number): *Must be 8 to indicate a checkpoint*
* `c` (point): *Required.*
  * Initial center point of checkpoint.
* `r` (number): *Optional. Defaults to 0.0*
  * Rotation of checkpoint. Expressed in radians.
* `s` (size): *Optional. Defaults to (0.0, 0.0)*
  * Size of checkpoint.
* `soff` (point): *Optional. Defaults to (0.0, 0.0)*
  * Spawn offset. That is, the distance from the center of the checkpoint where a player will respawn. Also affects the location of the checkpoint image onscreen.
  
