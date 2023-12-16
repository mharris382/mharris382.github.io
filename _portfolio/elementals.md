---
title: Elementals
excerpt: "An action platformer centered on spellcasting and magic"
header:
  overlay_image: /assets/images/elementals/elementals-header-overlay-2.png
  overlay_filter: 0.25
  teaser: /assets/images/elementals/elementals-teaser_thumbnail.png
sidebar:   
  - text: "[Github](https://github.com/mharris382/Software-Engineering-Capstone){: .btn .btn--primary}"
  - text: "[Web Build](https://tyranicgoat.itch.io/elementals){: .btn .btn--primary}"
  - title: "Role"
    text: "Programmer, Project Manager, Game Designer"
  - title: "Project Info"
  - text: "Feb 2022 - May 2022"
  - text: "**Engine:** Unity"
  - text: "**Language:** C#"
  - text: "**Tools:**"
  - text: "Cinemachine plugin"
images_folder: "/assets/images/elementals/"
---

This was my senior capstone project. I had made a simple platformer game during the second annual Go Godot jam and shared it with my capstone team suggesting that we remake and improve it in Unity. I wanted to finish the gameplay loop that had been prototyped during that jam.

{% include video id="qbkzEemPbv0" provider="youtube" %}

# Concept

The gameplay concept was an action platformer centered on spellcasting and magic. The player would control one of 5 elements and fight elemental enemies. Enemies drop mana when they are defeated, which the player uses to collect elemental mana to fuel spells. All elements use a shared supply of mana, but the player can only collect mana matching their current element. The original jam entry was missing a number of key features: it only had one level, no sound, there were no win/lose conditions implemented, and no enemy pathfinding. 

The project objectives were to:

1. Reimplement the game in Unity 
2. Add the missing game features
3. Expand and improve the game mechanics 

# Technical Highlights

This project was based on a previous project, my portfolio fighting platformer. The complexity of the combat system was reduced slightly to accommodate the ability of the other more junior programmers; but the core architecture was brought over. A key architectural design decision was to separate Monobehaviours into two categories: systems and data. The data components would not contain any (or very limited) methods or functionality.

<figure>
	<a href="{{page.images_folder}}character-movement-system-diagram.png"><img src="{{page.images_folder}}character-movement-system-diagram.png"></a>
	<figcaption>Character movement system diagram.</figcaption>
</figure>

The diagram shows how the character movement system was implemented using this shared data component pattern. The yellow class, `CharacterState` (MonoBehaviour) is the data container component. In this example it also has two subcontainers (regular classes) for specific sets of data. Specifically the data holding the desired movement state (Input) and the data holding the actual movement state. The blue classes (MonoBehaviour) are systems that either read and/or write to data held by the `CharacterState` component.  
There are several valuable points to notice about this diagram. First, none of the blue systems classes reference each other at all. `CharacterMove` only needs to know that the input data exists, it doesn’t know or care how it is being updated.  
Secondly, it’s important to realize that each data element is only written from one class within a system. I added notes to the movement state so it would be clear that reads and writes are not in conflict with each other between the `CharacterAnimator` and the `CharacterMove`. `CharacterAnimator` only writes to certain data that `CharacterMove` reads from.  The data that character movement writes to, is read-only for the animator. You can see this in the code snippet below showing the animator’s update function. This rule makes this system easy to understand and debug because there is always a single point of authorship for any piece of data. 

~~~ cs
void Update()
{
    //animator reads only from character movement state 
    var moving = Mathf.Abs(_state.Movement.Velocity.x) > 0.1f;
    _anim.SetBool(Moving, moving);
    _anim.SetBool(Grounded, _state.Movement.IsGrounded);
    
    //animator writes to interacting state from the animation machine parameters 
    _state.IsInteracting = _anim.GetBool(IsInteracting);
    _state.PhysicsMode = (InteractionPhysicsMode)_anim.GetInteger(InteractionPhysMode);
~~~

Additionally the `CharacterAnimator` writes to data inside its `OnAnimatorMove()` function to store root motion information. It’s important that the animator does not apply the root motion directly, because that would mean velocity is being authored in two places.

~~~ cs
private void OnAnimatorMove()
{
    var deltaPosition = _anim.deltaPosition;
    _state.Movement.AnimatorDelta = deltaPosition;
    _state.Movement.AnimatorAccel = deltaPosition / Time.deltaTime;
}
~~~

`CharacterMove FixedUpdate()` method showing how the movement system’s behavior is controlled by the data written by the animator. This allows the animator to take control of the character’s movement during important animations like spell casting.

~~~ cs
// Based on Physics Updates
private void FixedUpdate()
{
    void CompensateForMovingPlatforms()
    {
        if (State.MovingGround.Count > 0)
        {
            Vector2 velocity = Vector2.zero;
            for (int i = 0; i < State.MovingGround.Count; i++)
            {
                velocity += State.MovingGround[i].Velocity;
            }

            _rb.velocity = velocity + _rb.velocity;
        }
    }
    
    if(!IsJumping && IsGrounded)
        CompensateForMovingPlatforms();
    
    //if not playing animation do normal movement
    if (!_state.IsInteracting)
    {
        NormalMovementFixed();
        return;
    }
  
    //if playing animation perform movement based on interaction physics mode
    var rootMotionAccel = State.Movement.AnimatorAccel;
    switch (_state.PhysicsMode)
    {
        case InteractionPhysicsMode.FullRootMotion:
            _rb.velocity += rootMotionAccel;
            break;
        case InteractionPhysicsMode.Mixed:
            _rb.velocity += rootMotionAccel + (Physics2D.gravity * Time.fixedDeltaTime);
            break;
        case InteractionPhysicsMode.FullPhysics:
        default:
            break;
    }
}
~~~

The advantages of the systems described above are really that they are extremely flexible.  Any system can be removed or replaced with no impact on any other systems. New functionality can be integrated into the system with relative ease. For example the movement logic could be replaced with a full state machine implementation without needing to change any animator code or input code. The input can be replaced with an AI decision maker with absolutely no changes to the rest of the systems. This loose coupling made it much easier for the less experienced programmers to be able to dive into specific systems without needing to worry about all the extraneous logic and data required by every system in the controller.

# Challenges and Solutions

Bridging the skill gap between myself and the other two programmers was not an easy task. I had to implement most of the primary systems myself, but I wanted the other two CS students to get opportunities to learn and contribute to the project. Students were assigned to groups based on skill level, attempting to achieve a balance of advanced and beginner skill levels on each team. Initially our group was assigned with four members. I was assigned as one of the advanced members along with another programmer, leaving two remaining beginner students.  Unfortunately, the other advanced programmer left our team leaving me as the only advanced programmer on the team. Having never worked on a project of this scale, it was a significant challenge making the work accessible to all team members without taking them completely out of the programming tasks. I had to find a solution that would allow me to implement the more sophisticated systems without those systems interfering with and overwhelming the junior programmers.

Implementing the loose coupling described above allowed me to give the junior programmers systems to implement without them needing to concern themselves with the complex ramifications of the related systems. However, we still had to rely on some peer programming where I could coach them for some of the problems, especially at the very beginning. I tried to find as many avenues for the other programmers to contribute and develop the project beyond programming directly. I had to record a number of tutorials for team members to reference as well when working on various tasks. By the end of the project, each person had contributed at least one level that they had authored completely themselves and contributed numerous prefabs to the project, as well as contributing various amounts of code.

# Conclusion

This was my first taste of real project management. Before this, and some after as well, most projects had either been personal with no real deadlines or I was not in charge of leading and organizing the project. It was definitely taxing for me at the time, because I had to spend a lot of time working out ways for the inexperienced programmers to learn and be part of the project. At the end of the project I was slightly disappointed that I hadn’t gotten as much time to really experiment and iterate on the game mechanics. However, coming back to now I’ve already made some additions and improvements. Opening up after two years, I found the project quite easy to work with and easy to extend which is a testament to the overall quality and dedication to clean code.