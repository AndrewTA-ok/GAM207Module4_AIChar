Programmer: Andrew Todd Anderson Date: 07/22/2026
Milestone Three – AI Character

This milestone implements a basic AI wander behavior in Unreal Engine 5 using a Behavior Tree, Blackboard, and a custom Blueprint Task. The player character includes simple movement and firing to test AI interactions. This work follows the iterative documentation principles from Games, Design and Play, Chapter 7.

Blackboard
The Blackboard stores AI data. Only one key is required for this milestone:

Destination (Vector) – Used by Wander_BP and MoveTo.

Other keys (SelfActor, WanderLocation, TargetActor, LastKnownLocation) are placeholders for future modules.

Behavior Tree
The Behavior Tree runs a simple loop:

Wander_BP – Generates a random navigable point and sets Destination.

Wait (3 seconds) – Adds pacing.

MoveTo (Destination) – Moves the AI to the selected point.

This creates continuous wandering.

Wander_BP Task
Purpose: Select a random point on the NavMesh and update the Blackboard.

Key Fix:  
Get Owner caused runtime “Accessed None” errors because Behavior Tree Tasks do not have an Owner.
Replaced with Get AIController → Get Controlled Pawn, which provides a valid actor reference.

Node Flow:  
ReceiveExecute → Get AIController → Get Controlled Pawn → Get Actor Location → Random Nav Radius → Set Blackboard (Destination) → Finish Execute.

AI Controller (AICont_BP)
Minimal setup:

Event BeginPlay → Run Behavior Tree (BehaviorTree1)

This starts the AI’s Behavior Tree when the game begins.

AI Character (BP_AICharacter)
Health System: Responds to AnyDamage and destroys the actor at zero health.

Projectile Logic: Timer‑based projectile spawning (in progress).

Player Character (PlayerChar_M4_BP)
Includes basic controls:

Movement

Mouse Look

Jump

Fire (spawns M4 projectile)

Used to test AI behavior.

Projectile Material
A simple emissive material makes projectiles visible. Can be customized later for player vs AI bullets.

Troubleshooting
Issue: Runtime error from Get Owner.
Cause: BTTasks do not have an Owner.
Fix: Use Get AIController instead.





git push
Conclusion
This milestone successfully implements a functional AI wander system using Behavior Trees and Blackboard keys. It establishes a foundation for future AI features such as perception, targeting, and combat.
