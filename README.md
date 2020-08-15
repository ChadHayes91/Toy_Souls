# Project Description

Code & Executable: [https://www.dropbox.com/s/xj6134sxdskmit3/TeamOOBException_ToysSouls.7z?dl=0](https://www.dropbox.com/s/xj6134sxdskmit3/TeamOOBException_ToysSouls.7z?dl=0)

## Story Summary
Toy Souls is a prototype Dark Souls like game where the player is a child's toy whose owner has bought a new, secretly malicious toy (the antagonist) which perniciously plagues the minds of other toys. The antagonist, along with its possessed minions, continuously chases the player. The player will acquire skills and better gear throughout the adventure, culminating in a final showdown with this evil toy after the boss takes enough damage. However, the player will decide when it is time to eventually engage in combat, depending on how strong (or impatient) the player feels.

## NPC Behavior & Planning (AI)

This project was a group project for CS 6457 Video Game Design; my individual responsibility for this project were the NPC models, animations, and behavior. The 3D models used in this project were free assets from various websites, and the animations were created using Mixamo.
<br>
NPC behavior was programmed using a finite state machine; using an enumeration data structure to control the different states for an enemy NPC prefab. More specifically, the different possible states are as follows:
<ol>
  <li> Wander: an NPC agent picks a random location in a large radius of the player that is still within the NavMesh as a pathfinding destination (a destination is picked every predefined number of seconds), and the agent slowly walks to that location. If the agent arrives at the destination before the timer ends (and no other state is triggered), the agent waits at that location playing an idle animation. </li>
  <li> Chase: when the NPC agent is close to the player (using Euclidean distance between the two points: the player and agent), but not close enough to the player to melee him, the agent runs full speed towards the player (using NavMesh path planning). </li>
  <li> Shoot: the red NPCs will occasionally (with a cooldown) shoot a fireball at the player's current location. The projectile moves slow enough so that the player has enough time to move out of the way if the player moves immediately. </li>
  <li> Attack: when the agent reaches melee range of the player and the angle between the player and agent is acceptable, the agent tries to perform a melee attack. Afterwards, the agent returns to the chase state. </li>
</ol>

State transitions are almost all caused by a geometric test. For example, the test guaranteeing that the angle between the player and agent for the attack state is:
<br>

<ol>
  <li> Let vector A be the forward facing vector of the agent (Unity provides a function to get this data). Normalize vector A. </li>
  <li> Let vector B be the resulting vector after the subtraction of the player's current position and the agent's position (subtraction of two points). Normalize vector B. </li>
  <li> Calculate the dot product of vector A and B. </li>
  <li> The dot product is also equal to the magnitude of A times the magnitude of B times the cosine between the vectors A and B. Since vectors A and B both have a magnitude of one, the resulting dot product is equal to the cosine of the angle between vectors A and B. </li>
  <li> Since the cosine of zero (meaning there is no angle between the two vectors, or they're in the same direction) is one, if the result is greater than a value around 0.7, then the agent should enter the attack state. This means that the angle between the agent and the player is rather small.
  
</ol>


## Gameplay Video
