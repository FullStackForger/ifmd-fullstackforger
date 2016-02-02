```yml
title: Creating GameManager using singleton pattern
tags: [unity3d, singleton, game-manager, pattern, monobehaviour]
```
# Creating GameManager using singleton pattern

Here is another simple example of how you  game manager could be implemented with use of
`DontDestroyOnLoad()` and additional `isActive()` method if it derives from MonoBehaviour.

I [previous post][previous-post] we it was  explained how we can implement singleton pattern
and persist its instance after reloading the scene.
Now lets have a look at how we could take advantage of extending `MonoBehaviour`
class and using `DontDestroyOnLoad()`  method.

```cs
public class GameManager : MonoBehaviour {

	static GameManager _instance;

	static public bool isActive {
		get {
			return _instance != null;
		}
	}

	static public GameManager instance
	{
		get
		{
			if (_instance == null)
			{
				_instance = Object.FindObjectOfType(typeof(GameManager)) as GameManager;

				if (_instance == null)
				{
					GameObject go = new GameObject("_gamemanager");
					DontDestroyOnLoad(go);
					_instance = go.AddComponent<GameManager>();
				}
			}
			return _instance;
		}
	}
}
```

Example shown in [previous post][previous-post] provides easy access to all public properties
and methods of `GameManager` singleton retrieved via `GameManager.instance`  property.
This example however takes it to the next level,
as you can now add public properties which you can modify directly from within Unity3d Editor.

[previous-post]: /guides/unity3d/creating-game-manager-using-state-machine-and-singleton-pattern
