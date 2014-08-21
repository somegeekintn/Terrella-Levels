### Terrella World Description Format

---

This document explains the how to create and use world description files for Terrella. Terrella is a game created for iOS. Additional details can be found on [the Terrella web site](http://www.quietspark.com/terrella/). 

Terrella world description files are simply JSON files with a "fwd" file extension and so referred to as "fwd" files. The root object is a dictionary that must contain a "levels" element. "levels" is an array of level ojects which are documented below. 

Keep in mind that there is limited error checking for fwd files at the moment, and setting ridiculous values is likely to have ridiculous side effects.

##### Level (object)

---

* `arena` (bool): Optional. Defaults to false.
  * Indicates a multiplayer level. Should not be included in external fwd files unless set to false.
* `bonus` (number): Optional. Defaults to 0.
  * Starting value for this level's bonus.
* `desc` (string): Optional. Defaults to ""
  * The description for this level which appears in the flavor text display the first time a level is played.
* `hidebo` (bool): Optional. Defaults to false.
  * Indicates whether or not the blue and orange buttons should be hidden for this level.
* `setID` (string): Required.
  * The setID is used to group levels together in the level picker. This is also used as the set's display name for external fwd files.
* `worldID` (string): Required.
  * A unique identifier to this level.
* `worldName` (string): Optional (but strongly encouraged!). Defaults to "".
  * The display name for this level. Shown in the the level picker and elsewhere.
* `worldVersion` (number): Required
  * The version of this level. Changing a level in a fwd file will not necessarily update the world in game unless the `worldVersion` is different than the currently loaded version. 
* `objects` (array of object): Required
  * Described below
  