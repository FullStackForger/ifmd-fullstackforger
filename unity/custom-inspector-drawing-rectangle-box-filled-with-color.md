# Custom Inspector: Drawing rectangle box filled with color

```c#
/// <summary>
/// Draws plane rectangle filled with color
/// note it can only be called from OnGUI()
/// </summary>
/// <param name="position"></param>
/// <param name="color"></param>
public static void DrawRect(Rect position, Color color)
{
    Texture2D rectTexture = rectTexture = new Texture2D(1, 1);
    GUIStyle rectStyle = new GUIStyle();        
    rectTexture.SetPixel(0, 0, color);
    rectTexture.Apply();
    rectStyle.normal.background = rectTexture;
    GUI.Box(position, GUIContent.none, rectStyle);
}
```

<!--
References:
 - http://forum.unity3d.com/threads/draw-a-simple-rectangle-filled-with-a-color.116348/
 -->
