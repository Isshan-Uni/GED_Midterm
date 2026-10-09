# GED_Midterm
For this practical midterm, I was tasked with recreating the mechanics of Bubble Bobble. Due to the limited time, I focused on implementing the basic gameplay mechanics along with OOP, Singleton, and Factory design patterns.

The player attacks enemies to earn points, and defeated enemies fly upward before being destroyed. Enemies continuously spawn and chase the player. Once the player's score reaches 40, faster enemies begin spawning, increasing the difficulty. I also implemented a high-score system that keeps track of the highest score during the game session.

<img width="1262" height="769" alt="Screenshot 2026-10-08 210910" src="https://github.com/user-attachments/assets/d1c66755-34c2-4b87-9824-18bb7a7c0b77" />
<img width="1262" height="778" alt="Screenshot 2026-10-08 210918" src="https://github.com/user-attachments/assets/29048a10-5b89-4727-ad08-386ab52f83ca" />
<img width="940" height="405" alt="Screenshot 2026-10-08 210948" src="https://github.com/user-attachments/assets/cf60b635-fabc-422c-99ad-be188ab5ea7e" />
For the build you cannot see the attack area cause i did not have time, in the ue engine i am using the gizmo to show the attack area.
\
\
\
# Gameplay Features:
Enemy AI: Enemies use AI Move To to chase the player. The player loses if an enemy collides with them.
Player Attack: Uses a Sphere Trace to detect nearby enemies when attacking.
Slow Enemy: Requires one hit to defeat and changes color to red.
Fast Enemy: Moves faster and requires two hits. The first hit changes its color to orange, and the second changes it to red.
Enemy Death: Defeated enemies have their gravity direction changed upward, making them fly into the air before being destroyed after a short delay. This was inspired by Bubble Bobble's enemy mechanics.
Scoring: Each successful hit adds 10 points. Slow enemies give 10 points in total, while fast enemies give 20 points across two hits.
High Score: Tracks the highest score achieved during the game session.
Difficulty Scaling: At 40 points or higher, the Game Manager switches from slow enemy factories to fast enemy factories.
Win/Lose Conditions: The player wins by reaching 200 points and loses if an enemy touches them.
Win/Lose Menu: A separate level displays the result, final score, gameplay time, and high score.

# Object-Oriented Programming (OOP):

I used inheritance, polymorphism, and encapsulation to make the project more modular and avoid repeating the same logic across different Blueprints.

# Inheritance:
I created BP_EnemyBase as the parent class for both slow and fast enemies. It contains shared functionality such as AI movement, hit handling, color changes, and death effects.
Both enemy types inherit this functionality but use different movement speeds and hit requirements. This avoids duplicating the same logic in multiple Blueprints.

I also used inheritance in my Factory system, where the slow and fast factories are child classes of BP_EnemyFactory.
<img width="236" height="134" alt="Screenshot 2026-10-08 210047" src="https://github.com/user-attachments/assets/9448f190-d5bb-4e80-a5e5-70997777d6fb" />
<img width="242" height="134" alt="image" src="https://github.com/user-attachments/assets/a8f699d7-c134-4077-89e7-08855a5ed306" />


# Polymorphism:
I used polymorphism in both the enemy and factory systems.
In BP_EnemyBase, I created a DoEffect event that is overridden by the child enemies. The player calls the same event through a BP_EnemyBase reference, but each child enemy performs a different action.
Slow Enemy: Dies after one hit and changes color to red.
Fast Enemy: Takes two hits, changes to orange on the first hit and red on the second, awarding more points overall.
This demonstrates runtime polymorphism because the same event call produces different behavior depending on the enemy type.
I also created a shared ChangeColor function using a Dynamic Material Instance. Both enemies use the same function but pass different color values.
For the Factory system, the Game Manager accesses different child factories through their common parent type and calls CreateEnemy. Each factory has its own assigned enemy class, allowing different enemies to be spawned through the same function.

This makes the system easier to expand without rewriting the attack or spawning logic.
<img width="791" height="263" alt="image" src="https://github.com/user-attachments/assets/1b651873-797b-49c3-a4ae-c81e849f9870" />
CreateEnemy Function common between base and the child factoires.

<img width="610" height="208" alt="image" src="https://github.com/user-attachments/assets/5684fb73-1685-4bf6-b8a7-9b45688ec6f1" />
EnemyBase having a function which changes color of the mesh and child blueprints uses the same function with different value

<img width="110" height="71" alt="image" src="https://github.com/user-attachments/assets/b5673bc5-87e1-475d-83a5-035a0b411207" />
EnemyBase having DoEffect event which is empty each enemy child do there own effect like slow enemy dies on one hit fast enemy takes 2 and changes color twice and gives double score.

<img width="940" height="358" alt="image" src="https://github.com/user-attachments/assets/bac5e4da-9a77-4e40-980d-4b4b23cef843" />




# Encapsulation:
I used encapsulation in BP_GI (Game Instance) to manage the player's score, gameplay time, high score, and win/lose state.
Instead of accessing and modifying these values directly throughout the project, I created functions to control how other Blueprints interact with them.
AddScore, GetScore, ResetScore – Manage the player's score.
SetTime, GetTime – Update and retrieve gameplay time.
GetHighScore – Retrieve the highest score.
SetBwon, DidweWin – Set and retrieve the win/lose state.
The UI Text widgets use data bindings that call Game Instance getter functions to retrieve the score, time, and high score.
This allows both the gameplay HUD and Win/Lose menu to display the correct information without each widget having to manage its own copy of the data.
I chose this approach to keep the data management in one place and make it easier to update or modify later

<img width="195" height="356" alt="Screenshot 2026-10-08 205407" src="https://github.com/user-attachments/assets/5bf879e5-9c76-46c1-8538-41b988aec174" />
<img width="823" height="422" alt="Screenshot 2026-10-08 205732" src="https://github.com/user-attachments/assets/03ce45e3-b65a-42ec-83bf-1929386e112c" />




# Singleton Pattern – Game Instance

I implemented BP_GI, a custom Game Instance Blueprint, as a Singleton-style system for managing shared game data.
It handles:
Current Score
Game Time
High Score
Win/Lose State

I chose Game Instance because it provides a single shared instance that persists when changing levels. If I stored this information inside the player or a level-specific Game Manager, that information could be lost when transitioning to a new level.
Other Blueprints use Get Game Instance and cast to BP_GI to access its functions.

High Score System:
I also implemented a high-score system that compares the current score with the previous high score when transitioning to the Win/Lose menu.
If the current score is higher, it replaces the previous high score.
The high score is stored in Game Instance, so it remains available when changing levels during the current game session. It is not permanently saved after closing the application.
Both the gameplay HUD and result menu retrieve and display this information using the Game Instance functions.

<img width="195" height="356" alt="image" src="https://github.com/user-attachments/assets/be5a7269-0192-4586-bd63-bdc273d45818" />

When we win We set game instance to win for next level UI inside player charcter using function Set Bwon
<img width="725" height="193" alt="image" src="https://github.com/user-attachments/assets/52d6243f-0265-4d0d-bcc5-e5fc99a231cf" />


Win condition and level transition:

When the player meets the win condition, the Player Character calls SetBwon to update the win state in Game Instance before opening the Win_Lose_Menu level.
The result UI then retrieves this value to determine whether the player won or lost.


# Factory Pattern – Enemy Spawning

I implemented the Factory pattern using a parent BP_EnemyFactory and separate child factories for slow and fast enemies.
The parent factory contains a CreateEnemy function that spawns an actor using an Enemy Class variable rather than hardcoding a specific enemy Blueprint.
Each child factory assigns a different enemy class to this variable, allowing the same spawning function to create different enemy types.

Game Manager and Spawning Logic
I created a separate Game Manager to control when enemies spawn and which factories should be used.
On BeginPlay, a looping timer starts and calls SpawnEnemy every 3 seconds.
The Game Manager stores references to slow and fast enemy factories in separate arrays.
While the score is below 40, it loops through the slow enemy factories and calls CreateEnemy.
Once the score reaches 40, it switches to the fast enemy factories.
I separated these responsibilities so the Game Manager only decides when to spawn and which factory to use, while the factories handle which enemy class gets created.
This makes the system more flexible. If I wanted to add another enemy type later, I could create another child factory and assign its enemy class without having to rewrite the existing spawning function.

<img width="1272" height="141" alt="image" src="https://github.com/user-attachments/assets/e496ad0e-585f-4515-a655-5fd95b66c8ef" />
We assign different value to the child factory and then it spawns that base enemy child.

<img width="960" height="391" alt="image" src="https://github.com/user-attachments/assets/625207e5-0231-461c-83fc-2058fec9c9fd" />
Inside game manager it keeps spawning from the factory until we meet the score requirement.

# What I Wanted to Implement
I originally wanted to recreate Bubble Bobble's projectile mechanics more closely.
My idea was to have the player shoot a bubble projectile that expands when it hits an enemy. The enemy would become attached to the bubble and float upward, and shooting the bubble again would destroy the enemy.
I also wanted enemies to shoot projectiles at the player. For that, I was planning to use another Factory system to spawn different projectile types through the enemy base class.
Due to the limited time during the midterm, I focused on completing the main gameplay and making sure the OOP, Singleton, and Factory implementations were functional.
These are features I wanted to add if had more time.


I used spawning system from my lab activity 1 which used to spawn powerups but now it spawns enemy. I used change color mechanics from old project from class activity 1.
