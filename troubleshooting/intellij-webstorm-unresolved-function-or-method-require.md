# Unresolved method of function require

Problem occurs for nodejs projects in both IntelliJ IDEA and Webstorm.
Method `require` gets highlighted and if you roll over it you will get an error:
```
Unresolved method of function require
```

To fix it go to you project settings.

On MacOSX:
```
IntelliJ IDEA -> Preferences
-> Language & Frameworks -> JavaScript -> Libraries
-> [Ensure 'Node.js Globals' is checked]
```

On Windows:
```
File -> Settings
-> Language & Frameworks -> JavaScript -> Libraries
-> [Ensure 'Node.js Globals' is checked]
```
