# Keyboard `ScreenshotController` to grab different screen sizes

```c#
using UnityEngine;
using System.Collections;

public class ScreenshotController : MonoBehaviour
{
    void Update() {
        if ( Input.GetKeyDown( KeyCode.Alpha1 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize1.png", 1 );
        }
        if ( Input.GetKeyDown( KeyCode.Alpha2 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize2.png", 2 );
        }
        if ( Input.GetKeyDown( KeyCode.Alpha3 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize3.png", 3 );
        }
        if ( Input.GetKeyDown( KeyCode.Alpha4 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize4.png", 4 );
        }
        if ( Input.GetKeyDown( KeyCode.Alpha5 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize5.png", 5 );
        }
        if ( Input.GetKeyDown( KeyCode.Alpha6 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize6.png", 6 );
        }
        if ( Input.GetKeyDown( KeyCode.Alpha7 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize7.png", 7 );
        }
        if ( Input.GetKeyDown( KeyCode.Alpha8 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize8.png", 8 );
        }
        if ( Input.GetKeyDown( KeyCode.Alpha9 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize9.png", 9 );
        }
        if ( Input.GetKeyDown( KeyCode.Alpha0 ) ) {
            Application.CaptureScreenshot( "ScreenshotSize10.png", 10 );
        }
    }
}
```

<!--
reference: https://gist.github.com/bzgeb/6799103
-->