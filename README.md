### Terrella World Description Format

---

This document explains the how to create and use world description files for Terrella. Terrella is a game created for iOS. Additional details can be found on [the Terrella web site](http://www.quietspark.com/terrella/). 

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

An objects properties are dependent on the value of the `fc` propery which is an enumeration indicating the type of in-game object. Valid `fc` values are:

* `0`: *Orb*
* `1`: *Gate*
* `2`: *Goal*
* `3`: *Black Hole*
* `4`: *Player*
* `5`: *Round Wall*
* `6`: *Switch*
* `7`: *Wall*
* `8`: *Checkpoint*

Dimensions are in "meters" which span 20 points on screen at the normal zoom level. While not enforced, size and radius dimensions should be a multiple of 0.05 meters. Some typical values are:

* Smaller wall dimension: 0.4 meters
* Terrella's radius: 0.25 meters (or 10 points across at normal zoom)
* Typical goal radius: 1.6 meters

There is no expected origin or boundary for a level. The game will determine the bounding box for each level and transform all coordinates as needed.

Every object must also contain a point `c` which defines the object's center. Points and somtimes sizes are encountered frequently so let me define those before moving on to the specific types of game objects. 

##### Point

---

* `x` (number): *Required. x-coordinate*
* `y` (number): *Required. y-coordinate*

---

##### Size

---

* `h` (number): *Required. Height*
* `w` (number): *Required. Width*

---

The following describes the properties allowed for each object type based on the value of `fc`.

##### Orb

---


* `fc` (number): *Must be 0 to indicate an orb*
* `c` (point): *Required. Defaults to 0.0, 0.0.*
  * Defines an object's center.
