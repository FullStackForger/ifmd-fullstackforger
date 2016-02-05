# Deleting all children of an object

Deleting all children of an object might be confusing especially if you new to unity.  There are few ways to do that.

I tried deleting parent object hoping that it will remove children and it did work if I did that directly from the stage or code as long as there was no reference to that object in my GameProxy singleton class. Second disadvantage of that method is necessity of recreating parent object. Doesn’t take time but it is still extra line of code. It would be much better to loop through all the children and simply Destroy them.

I found quite useful conversation on Unity3d Forum in thread Deleting all children of an object. Tried few methods before finally realizing that it can be done with one line of code.

C#

```cs
foreach (Transform childTransform in parentObj.transform) {
    Destroy(childTransform.gameObject);
}
```

JavaScript

```js
foreach (childTransform in parentObj.transform) {
    Destroy(childTransform.gameObject);
}
```

Short and sweet.

However...

Those methods will work perfectly from within the game. However there is a problem with running it from within the Editor. Here is working version for `Application.isEditor()`

```cs
int children = container.childCount;
for (int i = 0; i < children; i++) {
	DestroyImmediate(container.GetChild (0).gameObject);
}
```
