---
title: Steam Eagles
excerpt: "2D crafting sandbox with a heavy emphasis on freedom and creativity, combined with physics systems and gas simulations."
header:
  overlay_image: /assets/images/steam-eagles/blurry-gear-background.png
  teaser: /assets/images/steam-eagles/SE_thumbnail_teaser.jpg
  show_overlay_excerpt: false
  actions:
    - label: "Itch Page"
      url: "https://tyranicgoat.itch.io/steam-eagles"
    - label: "Github"
      url: "https://github.com/mharris382/Steam-Eagles"
sidebar:   
  - title: "Role"
    text: "**Lead Programmer**, Designer, Project Manager"
  - title: "Project Information"
  - text: "Oct 2022 - May 2023"
  - text: "**Engine:** Unity"
  - text: "**Language:** C#"
  - text: "**Tools:**"
  - text: "UniTask, UniRx, Extenject (Dependency Injection Framework), Cinemachine"
images_folder: "/assets/images/steam-eagles/"
gallery:
    - url: /assets/images/steam-eagles/Recipe_Bench.png
      image_path: /assets/images/steam-eagles/Recipe_Bench.png
      title: Custom UI for creating the bench recipe
    - url: /assets/images/steam-eagles/Recipe_HGG.png
      image_path: /assets/images/steam-eagles/Recipe_HGG.png
      title: Custom UI for the Hypergas Generator machine recipe
    - url: /assets/images/steam-eagles/Recipe_SolidTile.png
      image_path: /assets/images/steam-eagles/Recipe_SolidTile.png
      title: Custom UI for the solid tile recipe
feature_pickups:
    - image_path: /assets/images/steam-eagles/machine pickup demo.gif
      excerpt: "Machine deconstruction"
    - image_path: /assets/images/steam-eagles/tile pickup demo.gif
      excerpt: "Tile deconstruction"
---

Steam Eagles is a 2D crafting sandbox with a heavy emphasis on freedom and creativity, combined with physics systems and gas simulations.  The project was sponsored as an independent research project at the University of Illinois. 

**Key Features**

- Custom Entity save/load system built using reflection and dependency injection
- Inventory and Crafting System built using the addressables API
- 2D platformer physics with ladders 
- Custom Building system with room tracking, room specific camera controls, and a custom editor integrated with a structured tilemap API

# Development Process


## Initial Game Jam

{% include video id="vO9i4VVp2zw" provider="youtube" alt="Video showing early prototyping iterations of gas sim mechanics" %} 

The inspiration for the project was as a week long game jam.  After the jam I spent time experimenting with gas mechanics in godot and found the result to be compelling. It was this prototype that I used to get the project sponsored as an independent research project at the University of Illinois.

## Optimization Learning

Early in the project, I realized that my early prototypes in godot would not scale to a large environment.  I wanted to understand how I could make this simulation on a large scale that would run in realtime.  This would be my first introduction to parallel programming, and I spent the first month of this project learning everything I could on the topic and how it could be applied to my gas simulation.  At this point I decided to switch to Unity since, at that time, godot lacked support for compute shaders. 

## Rapid Iteration and Experimentation 

After rebuilding all the basic mechanics in Unity as well as the gas sim, I focused on rapid iteration and experimentation.  There were only two blocks that the player could build at this point, solids and pipes; but that was enough to get the ball rolling.  From there I experimented with different ways of utilizing the gas simulation as a resource that the player could manipulate.  I added gas canisters that could be picked up and used to collect and release gas.  Full canisters could be used to power devices and machines in the level.  I found that often the most entertaining types of machines were ones that interacted with physics.

{::comment}
TODO: need to link video of this stage
{:/comment}

## Scaling Up

{% include video id="lT03U1JFnrQ" provider="youtube" alt="Result Video" %} 

Deciding to finally scale the project to a large open world was a big switch in the development process, and introduced a number of large hurdles to overcome.  Running the gas simulation on a large open world was out of the question, so I had to figure out a way around this.   

Furthermore, I wanted to have large movable airships players could build on and would contain gas. The solution to both of these challenges was the introduction of the building structure. There was a trade-off to the system, which meant that players would only be able to build within predefined areas; but I decided to embrace that constraint.  

The building system became an invaluable asset, and when I decided to switch to a dependency injection based architecture the building object became a clear entry point for many other systems such as crafting, saving, and cameras.

# Technical Highlights

## Scalable database driven Crafting System

{% include gallery caption="Examples of the custom recipe editor (click to enlarge)" %}

The crafting and item system was built based on database concepts taken from SQL that made it extremely flexible, scalable. Once the crafting framework had been implemented, it became relatively simple to add an endless number of new crafting recipes. Everything was linked using soft references (via the addressables API). Prefabs and items would only be loaded as needed. A recipe was a list of item stacks (a struct containing an item and a count, which was widely used throught the inventory API).  

## Pickups

{% include feature_row id="feature_pickups" %} 

Using the item’s name, I could load a pickup spawner. When a recipe was deconstructed I could therefore easily load the pickups that the object would drop when destroyed. The system was implemented generically so that it could be applied to prefab objects or tilemap tiles; both of which were important to facilitate a creative and flexible crafting experience. 

{::comment}
TODO: show gif of tiles and buildables being deconstructed and pickups gathered
{:/comment}

## Save/Load System Integration

The heavy use of soft references also served to make save data much more reliable. Initially I implemented save data by referencing the unity generated asset guid; however I found that this guid was prone to changing unpredictably. Addressable keys on the other hand were completely constant.  

Each room was saved independently, allowing rooms to be saved and loaded in parallel with each other.  Machines and prefab type buildables would be loaded separately. It also meant that the save data was much smaller in size.  

## Tilemap Save Data Compression Algorithm

I also devised a unique compression algorithm for saving the tilemap information, using grayscale textures to store the positions that contained tiles, avoiding the need to store empty positions (which was the majority of positions). The result was that a very large world could be saved and loaded very quickly.
