---
layout: project
title: Dreamwalker
excerpt: "A journey across mutable dreams"
header:
  teaser: /assets/images/dreamwalker/dreamwalker-thumb.png
  overlay_image: /assets/images/dreamwalker/dreamwalker-header.png
  overlay_filter: 0.25
sidebar:
    - title: "Role"
    - text: "Lead Programmer, Technical Artist, Level Designer, Modeling, and Project Manager" 
    - title: "Project Information"
    - text: "Nov 2023 - Current"
    - text: "**Engine:** Unreal Engine 5.3"
    - text: "**Language:** Blueprints, Material Nodes"
    - text: "**Tools:**"
    - text: "PCG Framework, Gaia, Blender, Miro, Google Sheets"
images_folder: assets/images/dreamwalker/
feature_pcg_holes:
    - image_path: "/assets/images/dreamwalker/pcg-holes.png"
      alt: Procedural holes creation
      excerpt: "I also added code in the floor that accounts for holes, which can be set to actually cut holes or to force specific sections to spawn as dirt or bricks."
gallery_pcg:
    - url: /assets/images/dreamwalker/pcg-stone-1.png
      image_path: /assets/images/dreamwalker/pcg-stone-1.png
    - url: /assets/images/dreamwalker/pcg-stone-2.png
      image_path: /assets/images/dreamwalker/pcg-stone-2.png
    - url: /assets/images/dreamwalker/pcg-stone-3.png
      image_path: /assets/images/dreamwalker/pcg-stone-3.png
gallery_dialogue_blueprints:
    - image_path: /assets/images/dreamwalker/dialogue-datatable.png
      url: /assets/images/dreamwalker/dialogue-datatable.png
    - image_path: /assets/images/dreamwalker/dialogue-blueprint.png
      url: /assets/images/dreamwalker/dialogue-blueprint.png
feature_dialogue_google:
    - image_path: /assets/images/dreamwalker/dialogue-google-sheets.png
      url: /assets/images/dreamwalker/dialogue-google-sheets.png
    - image_path: /assets/images/dreamwalker/dialogue-ue-database.png
      url: /assets/images/dreamwalker/dialogue-ue-database.png
---


Dreamwalker is a first person action adventure game, set in the world of dreams. I was attracted to this project because the opportunity to build worlds that don't fully adhere to the laws of reality.

{% include video id="-D9cks-ZmRI" provider="youtube" %}

# What Did I Contribute?

If you aren't interested in diving into all the nitty gritty details below and just want a list of my contributions to the project, read on. 

While this has been and still is very much a team project, I have been the main force behind 90% of the unreal engine work for this project. I am steadily onboarding other teammates in using unreal, but I am the guy who makes sure everything works, looks good, feels good, and gets finished on time.  

# Top Technical Contributions

- **Smooth and intuitive character movement**, expanding the built-in movement handling including climbing, ledge grabs, step ups, and swimming. The majority of which are also animated
- **Procedural Generation Tools** for rapid and non-destructive level building using PCG
- **Data driven Spore Effect Framework** which provides a simple yet elegant way to build and apply temporary special effects to the player (or other actors).
- Iterated and developed **intutive and flexible designer tools** to streamline development and produce fun and exciting gameplay
- **Full controller support** for all UIs using the Common UI plugin
- Integrated and customized a marketplace dialogue plugin

## Other Contributions
- **I designed the majority of the gameplay mechanics**, usually presenting them to the the team through rapid prototypes
- **Teaching and onboarding** team members in Unreal Engine

If it's in the game, I probably helped make it. In-engine audio is the only one area of development where I am not needed as frequently. However, there are a few standout features that I want to highlight as my most impressive contributions to the project.

# Procedural Tools Using PCG
Early in the project, when the game was still planned as a simple exploration driven walking simulator, I wanted to really make the world stand out in its size and scope.  

## Procedural Mushroom Forest
This mushroom forest is one of my favorite personal achievements on this project. It was the challenge of this forest which brought my skills with PCG and procedural generation to the next level. The prior level, the paper desert, was signifantly less detailed than a photorealistic mushroom forest scene. To limit the scope of this task, I gave myself the limitations of not using any trees. I approached this challenge by reverse engineering the PCG sample project (Electric Dreams). I attempted to translate the techniques used to create that environment to the one I was trying to create. I also worked with the audio engineer to make it so that the forest would automatically generate correctly placed audio for the rivers and lakes.

## PCG Designer/Artist Tools

I realized through making this forest scene that I could use PCG to build designer tools, that produced closer to final quality visuals using basic controls like splines and exposed blueprint properties. The forest level has a planned temple area at the climax. I continued iterating on the procedural temple wall, and then eventually added a procedural temple floor as well. The addition of the procedural hole actor provided a sculpting mechanism, essentially allowing designers to subtract bricks from PCG temple. I added this for the purpose of adding doors, but discovered it has many more uses as well. I also added code in the floor that accounts for holes, which can be set to actually cut holes or to force specific sections to spawn as dirt or bricks.

{% include gallery id="gallery_pcg" caption="Procedurally generated stone rooms" %}

### PCG Tools Feature Highlights
- Procedural Temple Wall (Spline & Grid Versions)
    - Randomness controls to achieve different styles
    - Designers can use procedural hole actor to cut holes in the wall
    - Optionally spawn vines on the outside of the wall
    - Optionally spawn a roof (spline version only)
- Procedural Temple Floor
    - Randomness controls to achieve different styles and moods
    - Controls for spawning dirt and mushroom detailing
    - Optionally snap to landscape
    - Optionally use bricks at edge 
- Procedural Plank Walkway (Spline)
    - **Uses a separate mesh for collisions**, ensuring smooth collisions, even if the planks look bumpy and uneven. Collisions are hidden in game and can be oversized to make certain objects more forgiving for gameplay than they physically appear

# Expanded Character Movement
Originally the game was meant to be a walking simulator, but as it matured into an adventure 3D platformer so did the player's movement capabilities. Early on I added sprinting and crouching, and then later added the ledge grab, climbing, and swimming. Of these, I am most proud of the ledge grab, as it has the most impact on gameplay. I built both underwater full freedom swimming and surface only swimming, but the level designer wanted to only use the surface swimming mode.

## Animations and Head Bobbing
Once I added animations, I realized that I could achieve a more compelling player experience by having the camera follow the character's head movements. This worked well, except some players found this motion uncomfortable. To make this feature optional, I gave the camera two possible targets. One was fixed to the skeletal mesh's head, the other was not. Then I could choose which to use based on whether that setting was enabled or not. 

## Ledge Grab and Ledge Climb
This was a feature that I implemented myself first. I did also check out some other implementations online as well, but I ended up mostly using my own version. This feature was one of the more complex mechanics that I built for Dreamwalker.  

I always strive to minimize the number of level designer steps, so automatically the grab will work on any valid collision surface. However, I did add the `"No Grab"` tag which can be added to *either* an actor or a specific component and it will prevent ledge grabbing on those objects.

Getting the climbing to feel *just* right took a few iterations. Eventually, adding animation combined with scripted movement allowed for the grab to feel intense, but also fully response. 
Grabbing dynamic platforms was slightly trickier. I tried a few techniques to accomplish this, but the most successful was to attach dummy actors to the grabbed components. This allowed the grab to react to any kind of platform transformation. Some objects are not grabbable, or have states where a grab is not valid, so to I used the `"No Grab"` tag to indicate platforms that could not be grabbed. If this tag is added during a ledge grab or ledge climb, the grab will be terminated.

[TODO: show gif on moving platform, scaling platform, rotating platform]: #

## Player Position Resets & Fall Height
I recently added a long fall system into the game which triggers a player reset if the player falls from a tall enough height. A nice feature about this system is the way it determines where to reset the player. The reset position system works by tracking the player's current location on the ground and recording the last valid ground location. Actor tags are used to mark certain platforms as "No Save", meaning the reset system will not count that platform as a valid reset. This allows designers to the force player to complete challenges without making a mistake, and will ensure they are sent back to the start if the player triggers a reset. This system also doubles as the player reload position used by the save/load system. All dynamic platform blueprints are already marked with the "No Save" tag, meaning that little or no designer setup is needed to create a particular challenge.

{% include figure popup=false image_path="/assets/images/dreamwalker/Fall_Height_Gif.gif" alt="Player falling and resetting" %}

### Fall Height System
The fall height system works by tracking the player's z position in the air (relative to the direction of gravity). Tracking the heighest point of the fall, the system can then determine how long the fall is in terms of height. To compensate for any dynamic elements such as low gravity or a bounce mushroom, the system sets the heighest point of the fall to the player's current position.  

{% include figure-modal.html src="/assets/images/dreamwalker/fall-blueprint.png" caption="Blueprint for resetting the player's position after a long fall" %}

# Save-Load System
Compared to some of my other projects, namely Steam Eagles, the amount of information required for save/load is relatively small. However, it is not-negligable. As an adventure game with a story driven progression system, there are many elements in the world which change as a reaction to events in the story. We need to load the player back to their exact stage in the story, so everything in the world is in the correct state. A few objects are tracked explicitly in the savegame class, such as the player, all NPCs, and certain interactables like levers. The rest of the game was saved as world events.

## World Event System
The World Events system saves everything implicitly by way of a unique world event, which is simply a name. The World Events system acts as both a global message bus, but also as a persistence mechanism. The WorldEventListener interface exposes a useful API for implementing any actor that needs to react to a specific world event.

~~~ cpp
class PATCHWORK_API IWorldEventListener
{
    GENERATED_BODY()
public:
    //Called when the event occurs in the world during gameplay. Use to trigger cinematics, animations, and gradual state change.
    UFUNCTION(BlueprintCallable, BlueprintNativeEvent, Category="World Events")
    void OnWorldEventOccured(FName worldEvent);

    //Called when the game loads to a point after the event has already occured. Use to set the state directly to it's end point without playing animations
    UFUNCTION(BlueprintCallable, BlueprintNativeEvent, Category="World Events")
    void OnWorldEventLoaded(FName worldEvent);

    //defines the set of world events which this class is concerned with.  
    //If left empty the listener will recieve all world events, otherwise it will only receive callbacks for the world events specified in the array
    UFUNCTION(BlueprintCallable, BlueprintNativeEvent, Category="World Events")
    TArray<FName> GetRelaventEvents();
}
~~~

**Note:** This project uses Blueprints, but I'm using C++ here for space and readability. 
{: .notice--primary}

## Unlocking Abilities
The mushroom bounce ability is the only currently implemented ability, but there are two more planned. It isn't a huge number of abilities to accomidate, so I didn't want to overcomplicate it. The abilities become unlocked after certain player milestones. I implemented an `ability_base` as an actor component so they could be activated by the player by listening for a specific world event (i.e. "Ability 1 Unlocked"). 

### Bounce Mushroom Ability

[TODO: Gif of bounce mushroom being used.]: #

The mushroom will stick to any surface except those marked with the "No Mush" tag

[**TODO: Gif of bounce mushroom thrown against a tagged surface and not working**]: #


# Data-Driven Gameplay Effect System
The system provides a single access point through a data table for each effect's behavior, timing, visuals, and post-processing (player only visuals).

{% include figure-modal.html src="assets/images/dreamwalker/spore-effect-datatable.png" alt="Spore Effect Datable" %}

The spore effect system plays a central role in the game's unique and surreal gameplay. The two driving forces in the effect system is the `Spore_Effect_base` and the `Spore_Region`. The effect's current status is tracked as a float called `toxicity` on the spore effect base. The region class tracks overlapping actors and increases toxicity over time. The effect itself will decrease toxicity over time, provided it is not currently overlapping a spore region. 

While it is a simple system, it can be expanded in limitless ways through new effect subclasses. 

## System Visuals
The other aspect of the spore system are the visual layers. There are two primary elements to the view layer: 
1. Visually distinguish the types of spores from each other
2. Applying visual effects while the player has the effect active

[SCREENSHOT OF ALL EFFECTS SIDE BY SIDE]: #

[SCREENSHOT OF ALL POST PROCESSING EFFECTS]: #

The first part has the additional complexity of potentially being represented in different ways depending on the context. 

{% include figure popup=true image_path="assets/images/dreamwalker/spore-visual-decoupling.gif" alt="Spore Visual Decoupling" %}


## Spore HUD Element
After playtesting we agreed that the player should also have a HUD element that can show all their current active effects as well as how much longer the effect will last.

[SCREENSHOT OF HUD DISPLAYING MULTIPLE SPORES]: #

[POSSIBLY REMOVE CODE]: #
~~~ cpp
On Overlap Begin(actor):
    spore_type = handle.table.get_row(handle.row_name)
    spore_type_class = spore_type.behavior_class
    
    //check if spore was instantiated on this actor by another region
    spore_class_inst = actor.get_component_of_class(spore_type_class)
    if(spore_class_inst == null):
        spore_class_inst = actor.add_component(spore_type_class)
    
    current_overlaps.add(actor, spore_class_inst)

Tick(delta):
    for each key_value_pair in current_overlaps:
        spore_inst = key_value_pair.value
        amount = compute_toxicity_delta_for_spore(delta)
        current_tox = spore_inst.toxicity
        current_tox += amount
        current_tox = clamp(0, 1, current_tox)
        spore_inst.toxicity = current_tox

On Overlap End(actor):
    if current_overlaps.contains(actor)
~~~

# Dialogue System plugin
I can't take complete credit for the dialogue system in this game, the majority of the system is provided by [this marketplace asset][1]. However, I did have to do a fair amount of work to integrate it properly. I also implemented my own customizations to improve the system. The plugin was provided as a full game kit not a standalone plugin, so I needed to reverse engineer much of the project to extract the code I needed.

[TODO: screenshot of the plugin in-game]: #

## Customizing the plugin
The biggest customization I made was the way that dialogue called outside code. The plugin sent the event string into a global message bus, and listeners would have to filter out for the string that they cared about. While this was good enough for some situations, I noticed that the behaviors called from inside dialogue usually pertained to the NPC speaking in the dialogue. So I added the ability to call functions/events that are defined on the speaker class, if the NPC class has a valid function name that function will be called as well. This uses a method of actor reflection as well, so it remains fully decoupled.  

{% include gallery id="gallery_dialogue_blueprints" %}

## Writing Pipeline: Easy Authoring for Writers
Once I felt I had a fairly comprehensive understanding of the plugin, I then needed to establish a pipeline for the writers to put their writing into the game. I acomplished this by creating a Google Sheet that I could then download as a .csv file and import into Unreal. 

{% include gallery id="feature_dialogue_google" type="center" caption="Converting a Google Sheet into an Unreal datatable" %}

# Menu System, Settings Menu, and Common UI Plugin
The dialogue tool I added depended on the Common UI plugin. Upon investigation, I decided that I should reimplement all the game's UIs to use this tool.

## Settings Menu
All the of game's settings are stored in a save file, separate from the gameplay save file. Settings are loaded immediately, and are globaly acessible by any class through the Game Instance. Each category of the settings menu is a widget that inherits from a base class. Using a dirty flag, the root menu class tracks whether or not settings have been altered. This allows the root settings menu class to prompt the player to either keep or discard their changes when they leave the menu. This feature is very important because players on lower end machines could set their settings too high, causing their frame rate to drop to near zero. If this happens, it becomes near impossible to navigate the menu and revert the settings. The prompt and the countdown timer solve this issue because it will automatically revert to the previous settings if the player fails to apply their changes.

[TODO: add a screenshot of the menu system]: #



[1]: <https://www.unrealengine.com/marketplace/en-US/product/defender-animated-dialogue-system> "dialogue system asset"
[2]: <https://www.unrealengine.com/marketplace/en-US/product/content-examples> "content examples project"