```yml
title: Creating Game Manager using state machine and singleton pattern
tags: [unity3d, singleton, game-manager, pattern, monobehaviour]
```

# Creating Game Manager using state machine and singleton pattern

Game Manager is quite broad topic and there are many ways to implement it.

From this article you will learn how to get the basic manager structure
with use of simple State Machine and Singleton pattern.

## Concept of GameManager – one object to control them all

Before we go any further we should consider what do we expect from our Game Manager and so it should:

- be easily extendible,
- have only one instance allowed,
- persist across all the scenes – shouldn’t be destroyed when new scene is loaded,
- contain list of available game states – so those can be accessed from other classes via ex.:  GameState.IntroScreen,
- store name of current game state – as above to enable accessing it externally with: GameManager.gameState,
- allow changing game state – again to expose it publicly via: GameManager.SetGameState(GameState gameState),
- trigger event when state changes – which will allow other objects’ modules to respond to that change with own handlers,

Additionally it  should:

- persist game progress – allowing user to continue after game re-opens after crash or close,
- save of current game progress – allowing manual saving progress of current level (game),
- restore desired game state – allowing manual load saved game.

List of requirements might be much longer but because it is quite broad topic
for now I will try to focus on those listed above. Game Manager can be extended later on.

## Singleton class – one instance and one instance only

First things first. Using Singleton Pattern lets make sure there is only once
instance of `GameManager` allowed at any given point in time

There are quite few different ways to implement singleton and I will not
go over all of them but here is the one that works great for me.
It does allow only one instance and protects from unwanted usage
displaying errors if constructor is being called in regular way.

```cs
public class Singletone {
    protected Singletone() {}
    private static Singletone _instance = null;

    public static Singletone Instance { get {
        return Singletone._instance == null ?
            new Singletone() : Singletone._instance;
    } }
}
```
Now if you try to create new Singleton instance using as you would create instance of regular object:
```cs
Singleton gameManager = new Singleton();
```

Unity will throw an error. You will see alert similar to the following:

```
Assets/Scripts/Singleton.cs(92,58): error CS0122: `Singleton.Singleton()' is inaccessible due to its protection level
```
If you look at the code again you will notice that class constructor
method has been followed by  protected keyword, which prevents from calling it.

The only way you can access singleton object is by referring to
its public static  property called  Instance

## Game Manager – long live and indestructible

 Lets modify our code a bit (it will make it easier to extend later on)
 and lets rename file to GameManager.cs too.

```cs
using UnityEngine;

public class GameManager {
    protected GameManager() {}
    private static GameManager _instance = null;

    // Singleton pattern implementation
    public static GameManager Instance {
        get {
            if (GameManager._instance == null) {
                GameManager._instance = new GameManager();
            }  
            return GameManager._instance;
        }
    }
}
```
Now Game Manager object should persist when we change, load new scene in game.

## Game States – publicly exposed

We could build dynamic State Machine with own customised interface for Unity Inspector allowing to change states on the fly assigning conditional behaviours for state transitions. We could… but we will NOT!

Goal is to create small robust code that can be easily re-used in other games, in fact… other small puzzle games targeted to deployed on web and mobile. Will described approach work for bigger projects? Yes! It will require more work though, which matter of fact should be easy as extendibility is one of the acceptance criteria for this project. So to keep all of that in mind lets create list of Game States adding it above Game Manager class definition, which will make it exposed.

```cs
public enum GameState { NullState, Intro, MainMenu, Game }
```

We also need gameState variable inside of Game Manager, so paste it above its constructor.

```
public GameState gameState { get; private set; }
```

## Game State setter – exposed too

Creating gameState property we decided to publish it with read-only access, making setting private. Aim was to use different method as a property setter and so now we can add it to our Game Manager class.

```cs
public void SetGameState(GameState gameState) {
    this.gameState = gameState;
}
```

## Delegate – as an elegant way of handling stuff

If you have some C# background not much has to be said but “delegate” topic might be quite new if you have experience in other languages and you might want to read up about it.

In lame man terms delegate is quite similar to a variable with the exemption it is made for functions. It allows other objects to perform += amd -= operations on delegate variable adding or removing own methods that will be triggered when delegate property function is called.

So what it does for us? It allows to handle triggered events by adding component handlers  method to a public delegate property. Lets see that in action. Firstly lets declare delegate above Game Manager class.

```cs
public delegate void OnStateChangeHandler();
```

Next we need to create delegate property for Game Manager.

```cs
public event OnStateChangeHandler OnStateChange;
```

And last thing is calling  `OnStateChange()` method from within the body of `SetGameState()` method.
Job done! Your `GameManager.cs` code (after cleanup) should look like this.

```cs
using UnityEngine;

public enum GameState { NullState, Intro, MainMenu, Game }
public delegate void OnStateChangeHandler();

public class GameManager {

    private static GameManager _instance = null;
    public event OnStateChangeHandler OnStateChange;
    public GameState gameState { get; private set; }
    protected GameManager() {}

        // Singleton pattern implementation
        public static GameManager Instance {
            get {
                if (_instance == null) {
                    _instance = new GameManager();
                }  
                return _instance;
            }
        }

        public void SetGameState(GameState gameState) {
            this.gameState = gameState;
            if(OnStateChange!=null) {
                OnStateChange();
            }
        }
}
```

## See it in action – example please!

Testing our simple Game Manager is quite simple. Create new stage, and attach below GameManagerTest.cs file to any object placed on that scene. And yes… you can attach it to the Main Camera. It is just a quick test.

```cs
using UnityEngine;
using System.Collections;

public class GameManagerTest : MonoBehaviour {

 GameManager GM;

 void Awake () {

   GM = GameManager.Instance;
   GM.OnStateChange += HandleOnStateChange;

   Debug.Log("Current game state when Awakes: " + GM.gameState);

   GM.SetGameState(GameState.Intro);
 }

 void Start () {
   Debug.Log("Current game state when Starts: " + GM.gameState);
 }

 public void HandleOnStateChange ()
 {
   Debug.Log("Handling state change to: " + GM.gameState);
 }

}
```

You should see those three expected logs in the console.
```
Current game state when Awakes: NullState
Handling state change to: Intro
Current game state when Starts: Intro
```

### Want some improvements?

In the [next tutorial][next-tut] I will show how to utilize Singleton pattern
for `MonoBehaviour` which becomes very useful especially
if you are planning to make it even more extendable.

[next-tut]:/guides/unity3d/creating-game-manager-using-singleton-pattern
