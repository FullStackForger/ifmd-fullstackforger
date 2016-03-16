# Auto destroying particle system in unity3d

There are few ways you can destroy particle system in Unity3d so question arises which one is actually the best?


## Scenerios of using particle systems

Very often you would need a particle system that is looping infinitely or playing more than once. In some cases, for example when one time explosion takes place you might want to clean the scene removing inactive particle systems. So lets have a look what would be the best way to achieve that.

## Prerequisites

It is is assumed you already have particle system added to the Scene. If not go ahead and create one.  Next step is to add particle controller, lets call it  `ExplosionController`

```cs
using UnityEngine;
using System.Collections;

public class ExplosionController : MonoBehaviour {

	void Start () {			
	}

	void Update () {			
	}
}
```

## Method 1: `Update()` and `IsAlive()`

The first option would be to destroy object when particle system finished emitting particles. It can be done with method  IsAlive() . Lets added to the body of Update()  method:

```cs
if(!this.particleSystem.IsAlive()) {
    Destroy(this.gameObject);
}
```

**Pros:** no spikes on your benchmark, system will use constant amount of resources to do the computation
**Cons:** computation (check) will be performed every single frame and even though it is simple check it will have to be run constantly, not the most efficient for high number of particle systems. Also it won’t work for short bursts of particles with “no particle time” between them.

## Method 2: `Update()` and `childCount()`

Another options would be to check number of children and when there is none destroy the object. That could look as below:

```cs
void Update () {
	if (transform.childCount == 0) {
		Destroy(gameObject);
	}
}
```

**Pros:** no spikes on your benchmark, system will use constant amount of resources to do the computation
**Cons:** as in previous example computation (check) will be performed every single frame but now might be also inaccurate when particles are generated in bursts, not the most efficient for high number of particle systems. And as previously… it won’t work for short bursts of particles with “no particle time” between them.

## Method 3: `StartCoroutine()` and `IEnumerator` `Autodestroy()`

There is one more way to destroy particle system. Method `Awake()`  is used to initialize any variables or game state before the game starts. It can be used to start "coroutine" destroying system after a period of time.

```
void Awake () {
	StartCoroutine(Autodestroy());		
}
IEnumerator Autodestroy() {
	yield return new WaitForSeconds(particleSystem.duration);
	Destroy(this.gameObject);
}
```

**Pros:** there is no checks every frame but one task scheduled to destroy particle system at the end of its duration
**Cons:** Might be not the most efficient for high number of particle systems

## Method 3+: Using `Object.Destroy()` in a right way

TI went back to documentation and found out `Destroy()`  method takes two parameters:
 - `object` - object to be destroyed
 - `delay` - time after which destroy action will occur

```
static function Destroy (obj : Object, t : float = 0.0F) : void
```

You can do something like this

```cs
void Awake () {
	Destroy(this.gameObject, particleSystem.duration);
}
```

**Pros:**  there is no checks every frame only delayed method destroying particle system at the end of its duration

**Cons:** Won’t work for particle systems without duration.

## Best Solution?

After doing some experiments I think there is no best way of handling particles. Solution depends on what you are trying to build.

Also bear in mind that if game has to deal with few or several particle systems it doesn’t really make massive difference which method will be used, however if we want to keep game optimized and being used resources (cpu power) low then we might want to test every single one to see what works best for you.

If you have particle systems systems that you want to be auto – destroyed after period of time use method 3. If you have set of particle systems and want to start disabling them after some time, or under some external conditions (ex. when user triggers some action) you might want call `Destroy()`  method externally.
