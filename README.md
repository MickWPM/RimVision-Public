# Rimvision
This is a personal toy project to load Rimworld save files (.rws) and walk around in first person. It started with a desire to just see through the eyes of a Rimworld pawn and ... will see where the scope goes from there!

![RimVision Pawn closeup](https://github.com/MickWPM/RimVision-Public/blob/main/Images/RimVision.png)

This is a very early build but enough people expressed interest that I wanted to get it out for people to play with over the holidays.

### Brief showcase (YouTube link)

[![RimVision showcase](https://img.youtube.com/vi/UwOMcUFtV3I/maxresdefault.jpg)](https://youtu.be/UwOMcUFtV3I)


## Current functionality
**Interaction**
- Walk around in First Person or fly around with free camera
- Walls/doors/fences/mountain collision prevents walking through (no collision in free camera mode)
- Doors are opened by interacting with them
- Spike traps function like bear traps (animation snapping shut, audio, disable player movement) - Bit of an easter egg!
- Roof and mountain visibility can be toggled on/off (to allow you to see inside bases and mountains)


**Generation:**
- Load save files (From local "Saves" folder and default Rimworld save location) and create base terrain. 
- Creates subset of "buildings" (Walls, doors, furniture etc)
- Roof all roofed areas
- Mountains
- Rock chunks (generic type only)
- Subset of vegetation (Jungle and Desert biome mainly) - scaled by plant growth level
- Pawns

## Controls
 
 If you have a lot of save files, the best way to "scroll" through them on the main menu is click and drag up/down. The user interface/experience will be overhauled down the track!
 
Quit - Alt-F4
Return to main menu - Escape

Swap between FREECAM/FPS view - P
Toggle roofs on/off - R
Toggle debug on/off - L
Record entry for degbug - Enter

FPS:
Movement - WASD
Sprint - Left Shift (hold)
Jump - Space
Interact (with green door) - F

Freecam:
Movement - WASD while holding RMB
Fast movement - Left shift (hold)

## Downloads
There are two builds available; both are **very** early. If you have a GitHub account please add any bugs etc through the **Issues** tab.

The only difference between the builds is the shader used for models; a build with the initial experiments with outline shading to break up monotonous textures is included but it may have performance impacts so both options are provided.

**RimVision (with outline shader)**

https://github.com/MickWPM/RimVision-Public/releases/download/EarlyDemoOutline/RimvisionPreview-Outlines.zip

**RimVision (basic)**

https://github.com/MickWPM/RimVision-Public/releases/download/EarlyDemo/RimvisionPreview.zip


## Screenshots

![Jungle base in 3D, isometric view](https://github.com/MickWPM/RimVision-Public/blob/main/Images/JungleBase.png)

![Farming base, top down no roof](https://github.com/MickWPM/RimVision-Public/blob/main/Images/FarmingBase.png)

![Mountain base](https://github.com/MickWPM/RimVision-Public/blob/main/Images/MountainBase.png)



### Outline shader 
The outline shader improves the flat look, especially indoors without effective lighting. 

**Base shading:**
![Indoors plain](https://github.com/MickWPM/RimVision-Public/blob/main/Images/Indoors.png)

**Outline shading**
![Indoors outline shader](https://github.com/MickWPM/RimVision-Public/blob/main/Images/IndoorsOutlines.png)



## Limitations/Known bugs
Debug logs output for each map in DebugLogs folder
If you find an error or want to flag something, enter debug mode (L), look at the relevant tile (visual indicator will show you) then press Enter.

Some missing ground textures (Largely DLC?)
Some missing plants (placeholder capsules)
Some missing equipment (eg. aircon/vent)
Mountains only use a single texture, regardless of rock/mineral type


## MODS

This only supports vanilla Rimworld by default however I have made RimVision moddable! This means you can add models/textures, both for missing items AND to improve the current (largely placeholder) models. Any item (vanilla or modded) that is found in the Mods folder will overwrite any existing item in base RimVision. **Please let me know if you add modded models!**

### Adding a mod
Mods are added in the "Mods" folder with the following structure:
- A folder for the mod entry (**MOD**) eg "Rimworld" for base Rimworld or the mod name for modded items.
- Folders inside here for the "thing type" (**CLASS**)
- .obj file within this folder with the modded item name (**DEF**)
ie.
Mods/MOD/CLASS/DEF.obj


### Included examples
Two examples are included:

**Rimworld**. An updated model (although equally placeholder) for the vanilla "Torch Lamp" object is included:

*Mods/Rimworld/Building/TorchLamp.obj*

The model TorchLamp.obj is loaded. As there is no material file it uses a basic untextured fallback material.

**MUR Wall Light**. (Included in _Washina.rws_) This is a modded item that is not included by default. Normally nothing would be rendered where MUR Wall Lights are located


### Getting information for mods
_Note save file location is by default:_

_C:\Users\User\AppData\LocalLow\Ludeon Studios\RimWorld by Ludeon Studios\Saves_

#### Modded Items
For items added in mods you will need to look inside the save file (\*.rws). This can be opened with a text editor (right click, open with) but I recommend Notepad++ as they are quite large files.

The top level filename (**MOD**) can be found at the top of the save, there will be a list of <modIds> (including the vanilla game as "ludeon.rimworld"). Use the relevant name here for your top level folder (**MOD**) inside the Mods folder. 
  
The **CLASS** and **DEF** names can be found within the <things> entries (there will be a **LOT** - this is all the things in the game). The **CLASS** name can be found in the entry _\<thing Class="CLASS"\>_. The **DEF** name is found underneath this _\<def\>DEF\</def\>_
  
As a full example using the Murmur Wall Light (included Mod example), the Washina.rws save file has the following:
  
  Line 25
```
  <li>murmur.walllight</li>
```
  
  Line 603340
  ```
            <thing Class="MURWallLight.WallLight">
						<def>Lighting_MURWallLight</def>
						<id>Lighting_MURWallLight3062754</id>
						<map>0</map>
						<pos>(106, 0, 146)</pos>
						<faction>Faction_11</faction>
						<questTags IsNull="True" />
						<parentThing>Thing_PowerConduit2728296</parentThing>
					</thing>
  ```

  As a result, the folder structure to add the Wall Light modded item becomes _Mods/murmur.walllight/MURWallLight.WallLight/Lighting_MURWallLight.obj_
  
  It is important that this convention is followed as it is how RimVision associates models in the Mod folder with the relevant entries in the save file.


#### Vanilla Items
The DebugLog folder includes logs for each map loaded. If a vanilla item cannot be found there will be an entry in here noting that it could not be spawned. This is in the form "Unable to spawn **CLASS**-**DEF**". To add mods to support vanilla items excluded, use the **Mods\Rimworld** folder and add "**CLASS**/**DEF**" to place the models in.

If you wish to replace an item that is already included (as per the Torch Lamp example), you will need to identify the CLASS and DEF names as per the modded items process above.

  ### Materials
  
  Currently this system only supports texture images; no other material properties are included. If there is a texture included, there needs to be a valid .mtl file (with the same name) and an entry for the image such as _map_Kd textureImage.png_
  
  _At the moment there needs to be a material file in the folder - this requirement will be removed in future but for now, ensure there is a .mtl file for each .obj file._
  
