NFR 3110 Make Up Assignment

Name : Isshan Marwah
Student Number : 100989890

# Object Oriented Programming
I created BP_FruitBase as the parent class for both apple and banana fruits. It contains shared functionality such as collision,, color changes. Both fruits types inherit this functionality but use different color variables and different amount of points. This avoids duplicating the same logic in multiple Blueprints.
I also used inheritance in my Factory system, where the apple and banana factories are child classes of BP_FruitFactory.

I used polymorphism in both the fruit and factory systems. In BP_FruitBase, I created a DoEffect event that is overridden by the child enemies. Each child enemy performs a different action.
Fruit: Adds 20 score
Banana: Adds 10 score
This demonstrates runtime polymorphism because the same event call produces different behavior depending on the fruit type. I also created a shared Color variables which lets use set different color switching option in the different fruit tyopes. For the Factory system, the Game Manager accesses different child factories through a array and chooses a random factory and callls CreateEnemy. Each factory has its own assigned fruit class, allowing different fruits to be spawned through the same function.
This makes the system easier to expand without rewriting the attack or spawning logic.


Encapsulation is being used in BP_GI (Game Instance taken from the midterm and modified little bit) to manage the player's score, gameplay time, high score, and win/lose state. Instead of accessing and modifying these values directly throughout the project, I created functions to control how other Blueprints interact with them. AddScore, GetScore, ResetScore – Manage the player's score. SetTime, GetTime – Update and retrieve gameplay time. GetHighScore – Retrieve the highest score. SetBwon, DidweWin – Set and retrieve the win/lose state. The UI Text widgets use data bindings that call Game Instance getter functions to retrieve the score, time, and high score. This allows both the gameplay HUD and Win/Lose menu to display the correct information without each widget having to manage its own copy of the data. I chose this approach to keep the data management in one place and make it easier to update or modify later.






Singleton Pattern – Game Instance
I implemented BP_GI, a custom Game Instance Blueprint, as a Singleton-style system for managing shared game data. It handles: Current Score Game Time High Score Win/Lose State
I chose Game Instance because it provides a single shared instance that persists when changing levels. If I stored this information inside the player or a level-specific Game Manager, that information could be lost when transitioning to a new level. Other Blueprints use Get Game Instance and cast to BP_GI to access its functions.
High Score System: I also implemented a high-score system that compares the current score with the previous high score when transitioning to the Win/Lose menu. If the current score is higher, it replaces the previous high score. The high score is stored in Game Instance, so it remains available when changing levels during the current game session. It is not permanently saved after closing the application. Both the gameplay HUD and result menu retrieve and display this information using the Game Instance functions.

It is being used in multiple place like in player to check if we reached the winning score in the fruits to add score and in the lose menu to showcase the score and time.

Factory Pattern – Fruit Spawning
I implemented the Factory pattern using a parent BP_FruitFactory and separate child factories for slow and fast enemies. The parent factory contains a CreateEnemy function that spawns an actor using an fruit Class variable rather than hardcoding a specific enemy Blueprint. Each child factory assigns a different fruit class to this variable, allowing the same spawning function to create different enemy types.
Game Manager and Spawning Logic I created a separate Game Manager to control when fruits spawn and which factories should be used. On BeginPlay, a looping timer starts and calls SpawnEnemy every 3 seconds. The Game Manager stores references different fruit factories in same arrays. While the score is below 40, it loops through the slow enemy factories and calls CreateEnemy. Every time it randomly chooses a factory and that factory spawns its assigned fruit. This makes the system more flexible. If I wanted to add another enemy type later, I could create another child factory and assign its fruit class without having to rewrite the existing spawning function.

