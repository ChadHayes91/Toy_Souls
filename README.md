<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

## Code, Project Description, and Story Summary
Code & Executable: [https://www.dropbox.com/s/xj6134sxdskmit3/TeamOOBException_ToysSouls.7z?dl=0](https://www.dropbox.com/s/xj6134sxdskmit3/TeamOOBException_ToysSouls.7z?dl=0)

Toy Souls is a prototype Dark Souls like game where the player is a child's toy whose owner has bought a new, secretly malicious toy (the antagonist) which perniciously plagues the minds of other toys. The antagonist, along with its possessed minions, continuously chases the player. The player will acquire skills and better gear throughout the adventure, culminating in a final showdown with this evil toy after the boss takes enough damage. However, the player will decide when it is time to eventually engage in combat, depending on how strong (or impatient) the player feels.

## NPC Behavior & Planning (AI)

This project was a group project for CS 6457 Video Game Design; my individual responsibility for this project were the NPC models, animations, sounds, and behavior. The 3D models and sounds used in this project were free assets from various websites, and the animations were created using Mixamo. 
<br>
NPC behavior was programmed using a finite state machine using an enumeration data structure to control the different states for an enemy NPC prefab. More specifically, the more interesting possible states (excluding things like idle and dead states) are as follows:
* Wander: an NPC agent picks a random location in a very large circle of the player's current location that is still within the NavMesh as a pathfinding destination (a destination is picked every predefined number of seconds), and the agent slowly walks to that location. If the agent arrives at the destination before the timer ends (and no other state is triggered), the agent waits at that location playing an idle animation. Having the wandering state's destination somewhat in the direction of the player's location guarantees that the NPCs won't get unlucky and the player never encounters any enemies. However, since the NPC's destination is inside a very large circle of the player's current location, NPCs will also not give the impression that the player is constantly getting chased.
* Chase: when the NPC agent is close to the player (using Euclidean distance between the two points: the player and agent), but not close enough to the player to melee him, the agent runs full speed towards the player (using NavMesh path planning).
* Shoot: the red NPCs will occasionally (with a cooldown) shoot a fireball at the player's current location. The projectile moves slow enough so that the player has enough time to move out of the way if the player moves immediately. This is done by instantiating a fireball object (which was created using Unity's particle system) with a constant velocity in the direction of the normalized vector created by the subtraction of the two geometric points: the player's center and the NPC's hands.
* Attack: when the agent reaches melee range of the player and the angle between the player and agent is acceptable, the agent tries to perform a melee attack. After attacking, the agent returns to the chase state.

Note that the behaviors outlined above are for the Teddy Bear NPCs; the behavior for the boss is similar, but all states are slightly different and some states are excluded entirely (like the wander state for example).

State transitions are almost all caused by a geometric test. For example, the test guaranteeing that the angle between the player and agent for the attack state is as follows:
<br>

* Let vector $$A$$ be the forward facing vector of the agent (Unity provides a function to get this data). Normalize vector $$A$$.
* Let vector $$B$$ be the resulting vector after the subtraction of the player's current position and the agent's position (subtraction of two points). Normalize vector $$B$$.
* Calculate the dot product of vector $$A$$ and $$B$$.
* The dot product is also equal to the magnitude of $$A$$times the magnitude of $$B$$ times the cosine between the vectors $$A$$ and $$B$$, or $$A \cdot B = \|A\|\|B\|cos(\theta)$$. Since vectors $$A$$ and $$B$$ both have a magnitude of one (because we normalized these vectors), the resulting dot product is equal to the cosine of the angle between vectors $$A$$ and $$B$$.
* Since the cosine of zero (meaning there is no angle between the two vectors, or they're in the same direction) is one, if the result is greater than a value around 0.7, then the agent should enter the attack state (assuming the agent is in melee range from a different test). This means that the angle between the agent and the player is rather small.

## Gameplay Video

#### Part 1 (main level, with teddy bear NPCs):

<iframe width="560" height="315" src="https://www.youtube.com/embed/UQ2wCOCAdTU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### Part 2 (final level, showdown with boss)

<iframe width="560" height="315" src="https://www.youtube.com/embed/y2TnqBdnWUU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
