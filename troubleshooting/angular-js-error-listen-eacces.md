# AngularJS – Error: Listen Access

If you trying to start your angular-phonecat web server with node.js by doing as described by Angular team you might find yourself staring at the error message. This post might help you out.
So lets see... If you execute below command as being told to...

```
node ./scripts/web-server.js
```

There is good chance after the first message, which is:
```
Http Server running at http://localhost:8000/
```

you will see error similar to the one below:

```
events.js:66
        throw arguments[1]; // Unhandled 'error' event
                       ^
Error: listen EACCES
    at errnoException (net.js:769:11)
    at Server._listen2 (net.js:892:19)
    at listen (net.js:936:10)
    at Server.listen (net.js:985:5)
    at HttpServer.start (D:\workspace\research\angular-phonecat\scripts\web-server.js:43:15)
    at main (D:\workspace\research\angular-phonecat\scripts\web-server.js:15:6)
    at Object. (D:\workspace\research\angular-phonecat\scripts\web-server.js:243:1)
    at Module._compile (module.js:449:26)
    at Object.Module._extensions..js (module.js:467:10)
at Module.load (module.js:356:32)
```

I am mostly Windows user but my guess is that if you on Linux you might experience it as well.
Issue is related to port number that node is listening on your localhost domain socket.
To fix the problem you should change the 8000 port to lower – not restricted one, like: 1234
open your web-server.js file and edit code around line  9, changing:

```
var DEFAULT_PORT = 8000;
```

to something like this:

```
var DEFAULT_PORT = 1234;
```
And give it a go again. That should sort out your problem.
