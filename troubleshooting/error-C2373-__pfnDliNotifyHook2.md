# error C2373: '\_\_pfnDliNotifyHook2'

Below error appears on Windows machine while installing `bcrypt`.

```  
error C2373: '__pfnDliNotifyHook2': redefinition; different type
 modifiers [E:\node\hapilizer\node_modules\bcrypt\build\bcrypt_lib.vcxproj]
```

To fix the problem update `npm`.
```
npm -g install npm@next?
```