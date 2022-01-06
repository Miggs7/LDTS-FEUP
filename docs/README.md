# LDTS_T12_G02 - Dungeon Boy

## Game Description


The Dungeon Boy is a Dungeon-Crawler like based game in which the player goes through different levels and maps and faces different enemies until reaching the final boss. It has 2 playing modes, the Survival mode in which the player has to survive through some tough fights with enemies, and try not to die along the way (at least not too many times).
The Player vs Player mode (PVP) consists in a mode where the player can face a player 2, and the winner will be crowned on the best results out of 5 intense rounds.

This project was developed by João Duarte (201707984), Miguel Tavares(202002811)   and Inês Garcia ( 202004810) for LDTS 2021-22.

**NOTE** : This game is still on development and could suffer some changes!

### GAME SUMMARY

The game starts with a black terminal with the name of the game, and some commands.

![image](https://user-images.githubusercontent.com/52889593/148138582-3bba6e0e-ec13-4da5-8398-d42c02853e32.png)

If we press ENTER, we are carried into the main menu, where we can select which game mode to play 

![image](https://user-images.githubusercontent.com/52889593/148138743-0b59bb66-2572-413a-b573-dadb35796689.png)

Survival Mode: We can now play against 4 different Enemies which will try to kill us!

![image](https://user-images.githubusercontent.com/52889593/148138798-9ade6cc1-4c5d-451a-97b8-3eecf5ab75fa.png)

PvP Mode: Play against your friend, best of 5 rounds win! Good luck!

![image](https://user-images.githubusercontent.com/52889593/148138866-38876123-19e6-4a9c-9567-150c9f001c57.png)


## Implemented Features

- **Connected Menus** - The user has the capability of browsing through the different menus including in game ones. (Ex: Intro Menu, Main Menu with mode selection screens, Instructions,  Shop(when dead) and ESC.
- **Keyboard control** - The keyboard inputs are received and interpreted according to the current game state.
- **Player control** - The player may move and attack the enemies with the keyboard control.
- **Collisions detection** - Collisions between the player and the enemies are verified.
- **Hit Points** - If the player collides or is attacked by an enemie, loses hit points. 
- **Diferent enemies** - The player will face different enemies throughout the game.
- **Different modes** - The player can choose between the Survival Mode and the Player vs Player mode.
- **Shop interaction and money management** - The player may buy new items in the in game shop, which consist of new weapons and potions.
- **Catching HP Potions** - If the player goes to the position of a potion, this one is going to be collected, adding 10 health points to his total HP.


### PLANNED FEATURES - Still being implemented!


- **Player control** - The player may move and attack the enemies with the keyboard control.
- **Collisions detection** - Collisions (still need to be implemented)
- **Diferent enemies** - The player will face different enemies throughout the game.  (still need to be implemented)
- **Shop interaction and money management** - (still need to be implemented)
- **Catching HP Potions** - If the player goes to the position of a potion, this one is going to be collected, adding 10 health points to his total HP. (still need to be implemented)
- **Final Boss** - (still need to be implemented)
- **Inventory** - There will be an inventory to store our items that we bought from the shop.
- **Arena Transition** - Player can pass to another new Arena

## Design


- **Problem in Context.** 

This game uses many classes, and we tried to connect many of them to the Game class, such as Arena, HP, BadGuy, Player and Wall.
We mainly tried to create this classes objects on Game/Arena.


- **The Pattern.** 

We adopted the Builder creational design pattern to construct complex objects step by step.
It was clearly used on our creation of the BadGuy (enemys), along with our Arena class which used the Builder on creating the players, and enemys(eggman's)

- **Implementation:**

Regarding the implementation, we now have classes which main purpose is to store data (model), classes that control the logic of the game (controllers) and classes that are responsible for the visual effects on the screen (viewers), these types of classes associate with each other in the following manner:

<p align="center" justify="center">
  <img src="images/UML/MVC.png"/>
</p>
<p align="center">
  <b><i>Fig 1. Model, Controller and Viewer pattern design</i></b>
</p>

As for the different states, they are divided with the same methodology as the mvc style, and permite the game to alter its behavior in a simple and efficient way.

#### Consequences:

The main pros of using this design pattern are: 
 - We could construct objects step-by-step, defer construction steps or run steps recursively.
 - We could reuse the same construction code when building various representations of products.
 - We could isolate complex construction code from the business logic of the product, Single Responsibility Principle. 

But however, the complexity of the code increased since we had to create multiple new classes.


### Observers and listeners
#### Problem in Context:  
Our game is controlled both by the mouse and the keyboard, many are the ways to receive input from these devices, for example: a thread that is running and every time it catches a signal it sends to the game itself (polling), the game being responsible for asking for input when needed, which is costly for our program since we may not send any signal to the program and unnecessary calls are made or the way we decided to implement which consists of using observers also known as listeners that are responsible for receiving the said input and redistributing it in a nicer and more usefull way to us. This takes some "weight" of the program as it will no longer need to be polling for input, as it will be properly warned when received.

#### The Pattern:
We have applied the **_Observer pattern_** which is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing. With this in mind the pattern allowed to solve the identified problems and apply a sustainable mechanism to receive any program input.

#### Implementation:
Implementation wise we store the observers in the main class (game class) and change its state according to the respective input processed by the available listener.
Though, it wasn't easy right from the start as our first attemp to implement this feature didn't act as expected. All listeners were always active, since when creating a Menu Button the listener would be activated by the newly created state, and it was far from being a structured and easy-to-read code.

<p align="center" justify="center">
  <img src="images/UML/ObsListener.png"/>
</p>
<p align="center">
  <b><i>Fig 2. Observers and Listeners screenshot</i></b>
</p>

#### Consequences:
Some consequences of using the stated pattern:
- Promotes the single responsibility principle.
- Clean code.
- Only the current game state is warned when an input is given.

### Battlefield builder
#### Problem in Context:
A battlefield consists in an aglomeration of elements such as walls, bombs, portals, a player, etc. 
Having different battlefields in the various levels, instead of having a builder to each level, a battlefield loader that reads from a file and inserts into its super class (battlefield builder) the needed elements, was the most appealing option. This implementation makes it possible to only create specific elements and also generate battlefields in different ways.

#### The Pattern:
The **_Factory Method_** and **_Builder_** are two creational design patterns, the first one provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. The second, allows you to construct complex objects step by step making a simpler code.

#### Implementation:
A factory is resposible for constructing the whole but the workers are the ones that actually execute the job. Analogously, our BattlefieldBuilder is a factory and its subclasses represent the workers.
As for the implmentation, the BattlefieldBuilder is an abstract class which knows how to construct a battlefield, however only its subclasses supply the necessary components of the battlefield. The BattlefieldLoader is one of referred subclasses that consists in a worker capable of reading different levels from different files. Another possible implementation would be a random loader that generates random components for the battlefield.
The builder pattern is implemented in all of the above classes by dividing the constructing in smaller steps.

<p align="center" justify="center">
  <img src="images/UML/BuilderLoader.png"/>
</p>
<p align="center">
  <b><i>Fig 3. Battlefield builder and loader</i></b>
</p>

These classes can be found in the following files:
- [BattlefieldBuilder](../src/main/java/com/g57/model/battlefield/BattlefieldBuilder.java)
- [BattlefieldLoader](../src/main/java/com/g57/model/battlefield/BattlefieldLoader.java)

#### Consequences:
Benefits of applying the above pattern:
- You avoid tight coupling between the creator and the concrete products.
- Open/Closed Principle. You can introduce new types of products into the program without breaking existing client code.
- You can construct objects step-by-step, defer construction steps or run steps recursively.


### Different types of commands
#### **Problem in Context:** 
In an initial and more simplified version of the current game, the diversity in between buttons was not significant. However, in the course of the development of the project, the number of buttons increased exponentially and the need to generalise the button element became more evident. That said and knowing that good software design is often based on the principle of separation of concerns, a major refactor had to be done. 

#### The Pattern:
We have applied the **_Command_** also know as Action pattern. This pattern turns a request into a stand-alone object that contains all information about the request.

#### Implementation:
Regarding the implemetation process, all the Button classes were deleted and transformed into a single **Button** with a command attribute. These **commands**  implement the same interface having an execution method that takes no parameters. This interface lets you use various commands with the same request sender, without coupling it to concrete classes of commands. As a bonus, now you can switch command objects linked to the sender, effectively changing the sender’s behavior at runtime.

<p align="center" justify="center">
  <img src="images/UML/ButtonCommand.png"/>
</p>
<p align="center">
  <b><i>Fig 4. Buttons and commands</i></b>
</p>

These classes can be found in the following files:
- [Button](../src/main/java/com/g57/model/element/button/Button.java)
- [Command](../src/main/java/com/g57/model/item/command/Command.java)

#### Consequences:
The Command Pattern allows the following consequences:
- You can decouple classes that invoke operations from classes that perform these operations (SRP).
- You can implement undo/redo.
- You can assemble a set of simple commands into a complex one.
- The code may become more complicated since you’re introducing a whole new layer between senders and receivers.


### GUI
#### Problem in Context:
Aiming for a structured and unstable (easy to change) code, we tried to make it as general as possible. The lanterna library contains various functions that aren't useful to our program, Interface Segregation Principle violation, and lacks some other functions that our interface needs. Also, if using the raw library, our game (high level module) would be directly depending on a low level module. This is a violation of the Dependency Inversion Principle (DIP). A need to implement an interface that solves these problems was born. 

#### The Pattern: 
We have applied the **_Facade_** pattern. A facade provides a simple interface to a complex subsystem which contains lots of moving parts, allowing us to only include the features that really matter.

#### Implementation:
<p align="center" justify="center">
  <img src="images/UML/GUI.png"/>
</p>
<p align="center">
  <b><i>Fig 5. Observers and Listeners screenshot</i></b>
</p>

These classes can be found in the following files:
- [Game](../src/main/java/com/g57/Game.java)
- [GUI](../src/main/java/com/g57/gui/GUI.java)
- [LanternaGUI](../src/main/java/com/g57/gui/LanternaGUI.java)

#### Consequences: 
The use of the Facade Pattern in the current design allows the following benefits:
- Isolate code from the complexity of a subsystem.
- Promotes testability and replaceability.
- Expand lanterna functionalities as well as respecting the Interface Segregation Principle.


## Known Code Smells And Refactoring Suggestions
#### **Large Class**
Some classes (e.g. Game, Battlefield, Player) contain many fields and others (e.g. GUI interface) contain many methods. In both cases, we find it justifiable as the classes require these fields, in one hand the Game class is the main class of the program and it needs to store a considerable amount of data, on the other hand various methods are needed for the interface and it wouldn't make sense to split it into two separate ones (extract method).

#### **Data Class**
All model classes are Data Classes, as they contain only fields, and no behavior (dumb classes). This is caused by the **MVC** (Model-View-Controller) architectural pattern which holds the responsibility to the controller to implement the logic functionalities of each model.
This is not a bad code smell because it only exits due to the chosen design pattern.

#### **Alternative classes with different interfaces and Lazy Classes**
When we conceived the project ideas, we aspired various enemy types with different behaviours. However, with the project development, we decided to generalize our **Enemy Class** and differenciate, the divergent characteristics, from contrasting enemies based on their fields. As this classes only differ in the values passed to the **Enemy Class** constructor and have no other significant functions they are an example of **Alternative Classes with different interfaces and Lazy Classes**.

#### **Refused bequest**
In an attempt to generalize and simplify our code, various abstract classes and interfaces were created. Nevertheless this resulted in the rising of the **Refused bequest** smell. As a result, some subclasses inherited methods from its parent classes which are neither defined nor used. For example, the [**SwapCommand Class**](../src/main/java/com/g57/model/item/command/SwapCommand.java#L31).

#### **Feature envy and message chains**
As the result of the **MVC** (Model-View-Controller) pattern some of the controllers use is narrowed to its model method calls. Our controller envies its model.
Also, in order to access a certain model's parameter it is mandatory to start by making a request to its controller.

## Testing

### Screenshot of coverage report
<p align="center" justify="center">
  <img src="images/screenshots/codeCoverage"/>
</p>
<p align="center">
  <b><i>Fig 6. Code coverage screenshot</i></b>
</p>

### Link to mutation testing report
[Mutation tests](../build/reports/pitest/202105302045/index.html)

## Self-evaluation

- João Duarte - %
- Miguel Tavares - %
- Inês Garcia - %