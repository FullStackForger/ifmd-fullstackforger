#"unresolved function or method require()" in IntelliJ IDEA

If you just installed IntelliJ IDEA or IntelliJ Webstorm there is good chance your IDEA doesn't recognise your node project. If you roll over underlined method name such us `require()` you will see a hint:  
___unresolved function or method require()___

To fix that problem open Preferences with `[cmd] + [,]` if you are on Mac 
or `ctrl + ,` if you are on Windows.

Navigate to `Javascript > Libraries` and enable:
- `Node.js Globals`
- `Node.js Core Modules`