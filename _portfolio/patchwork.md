---
title: Patchwork
excerpt: "[insert a tagline here]"
header:
  overlay_image: /assets/images/patchwork/patch-overlay-horizon.png
  overlay_filter: 0.25
  teaser: /assets/images/patchwork/patch-overlay-horizon.png
sidebar:   
  - title: "Role"
    text: Lead Programmer, Technical Artist, Level Designer, Modeling, and Project Manager 
  - title: "Project Info"
  - text: "Nov 2023 - Current"
  - text: "**Engine:** Unreal Engine 5.3"
  - text: "**Language:** Blueprints, Material Nodes"
  - text: "**Tools:**"
  - text: "PCG Framework, Gaia, Blender, Miro, Google Sheets (for authoring datatables)"
images_folder: "/assets/images/patchwork/"
feature_row:
  - image_path: /assets/images/patchwork/patch-overlay-horizon-2.png
    excerpt: "Customized Dialogue System Plugin with Save System and Questing"
  - image_path: /assets/images/patchwork/patch-overlay-horizon-2.png
    excerpt: "Open world procedurally generated landscape, each with unique artistic direction"
  - image_path: /assets/images/patchwork/patch-overlay-horizon-2.png
    excerpt: "Unique gameplay mechanisms based on experimental concepts such as hallucinations, distorted perceptions, and other mind and body altering"
  - image_path: /assets/images/patchwork/patch-overlay-horizon-2.png
    excerpt: "Numerous interactive NPCs that can react to player actions both in and out of dialogues" 
toc: true
toc_label: Table of Contents
toc_sticky: true
---

**Note:** This is an in-progress project. It will be a submission to the [LSD Dream Emulator Game Jam]{: style="color: blue"}.
{: .notice}

[LSD Dream Emulator Game Jam]: https://itch.io/jam/lsdjam-2023 "LSD Dream Emulator Game Jam"

 The project is based on an open-ended walking simulator. The game is experienced from a First Person or Third Person perspective. In the game, the player will travel into 3 dreamworlds where their objective is to find and save the dreamer who is stuck in the world.

# Technical Highlights

One major technical highlight is the procedural generation techniques that were used to create and populate a large open world in such a short amount of time. I was able to implement this using Unreal’s new Procedural Content Generation framework. Three weeks ago, when we started the new sprint based development cycles was the first time I used this framework, but I was so amazed at what I was able to accomplish with it; I wanted to incorporate it into the next world

# Development Process

Initially this project struggled with finding a direction, and the development process struggled because of that. We decided to explore the idea of building multiple worlds each with its own story and artistic style. This was a compromise to the team not being able to agree on a single visual style. Unfortunately with each team member only focusing on their own world, we were unable to make any consistent progress or reach a playtesting phase. 

Sensing that this loose unstructured development process was going to derail the project, I suggested in a meeting that we narrow the scope and only create one world at a time which we would all collectively focus on. The idea was that we would complete the entire level in the timespan of 2 weeks, and then move onto the next level. This process would be repeated for 3 levels, giving us a remaining 4 weeks to polish the game before delivery.

So far this development process has been a game changer. We have already completed the first 2 week long sprint, and successfully delivered an open world level with both an exploration segment and a puzzle/platforming segment.

We are currently half-way through the second world, which is quite a bit more ambitious than the first one in terms of gameplay and art style; but progress is coming along quite nicely.

# Challenges 

## PCG Framework

Working with the PCG framework has been extremely enlightening so far. I’ve learned about new more advanced workflows which allow PCG to reach new levels of generation. Particularly the packed level workflow, which allows you to author a scene or assembly by hand and then bring that level into PCG. The really cool thing is that once in PCG you can take procedural control over the handmade level and apply as much or as little randomness and variation as needed. This gives the benefit of having complete artistic control over the assets and being able to create complex realistic natural scenes, such as vines growing up trees (or mushrooms in this case) and adding variations that make the assemblies look less copy-pasted and more realistic. I’m sure that there is a lot more to be learned with PCG, but I’m already amazed at what I could accomplish in the first 3 weeks of using this framework.

## Diaglogue System

Implementing the dialogue system in such a short amount of time was challenging. Fortunately I did find a dialogue system on the epic marketplace, which was part of the free for the month collection. However, the plugin included more features than we actually needed for this project. Separating out the dialogue features from the rest of the framework took a bit of digging. One important feature that I needed to make the questing system work was the ability to trigger world events from the dialogue system datatables. This was implemented as a global event publisher on the Dialogue Actor. Instead I wanted to call the event on the NPC that was involved in the dialogue, in order to trigger specific NPC logic after critical dialogues. I modified the system so that it would execute a named event on the dialogue owner, allowing me to directly implement the event on the NPC pawn blueprint rather than have to go through the message bus. 

One problem with this system was that on its own the event could only be called through the dialogue system. However, if the player were to save the game and load back into the world, there are times when the event needs to be called again or the game would become unwinnable. For example, in one case the NPC was waiting for the player to collect 3 keys. At this point their dialogue state would be saved to the point after the NPC tells the player to go get the keys. The world state is then queried until the keys are found, once they are found the NPC will move it’s dialogue to the completion state and stop the query. However, if the event is not called when the game reloads the NPC will still be in the waiting state, but they will not start the query loop. Since the player is unable to reinitiate the dialogue that triggers the start of the query loop the player will never be able to proceed to the completion dialogue. This was solved by adding a custom function in the NPC base class which allows the NPC to save a dialogue event. If this is done the event will be recalled on the NPC whenever the game is loaded.

