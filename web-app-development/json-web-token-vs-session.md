# JSON Web Token vs Session

JSON Web Token is generated on the server but stored by the client application.
Token act as container storing information identifying authenticated user
along with authorization claims assigned to the user or its role.

Token is encoded using `base64` and can be decoded on the client.
However, token signature is encrypted with secret key stored on the server.
It means tokens are secured as they can't be forged on the client.
Only server can validate JWT signature. It can be done on fly so there is
no need to store tokens on the server. It is however possible and if done so,
server still have to check expiry date and auto refresh the token when needed.

## Better scalability

As server doesn't have to store state or session information it becomes
easier to scale. Load balancers can now redirect HTTP request without worrying
about either authentication or authorization.

Client can make request to any instance of scaled horizontally service passing
along JWT when needed. Token itself holds all authorization data required
to access restricted resources.

## More control

Using session usually means storing it on the client as a cookie, which then
is automatically attached to every HTTP request made from the browser.

Sending tokens, on the other hand, is controlled from application level.
That means application gains control over how HTTP requests are made
and what authentication information they contain.

## Stateless

Typical problem with session occurs when server fails or one of the instances
was shut down. In most cases application looses it's state and fails.

When that happens the load-balancer picks another server but some data is lost.
Typically cases user is forced to login again, potentially losing some data.
To mitigate the problem state may be stored in a database. That introduces
additional complexity to the system.

Because JSON Web Tokens are not stored on the server session affinity
or sticky session issues don't really exist.
Using tokens makes building stateless services easier.

## Improved security

Using cookies creates [Cross Site Request Forgery][1] vulnerability because
cookie is sent with every request. Using JWT helps to prevent CSRF attacks.
Even if for some reason application stores the token within a cookie,
the cookie is not used as an authentication mechanism.

Security with tokens can be improved even further by using `exp` claim holding
an expiration date. Expired token are invalid and force user to login again
to either obtain new valid token.

Additionally tokens can be revoked. That means invalidating a specific token
which will restrict user or third-party access until new valid token is generated.  

As for [man-in-the-middle][2] attack, like in any other situation request should
be made over HTTPS.

## Better cross-platform support

Using JWT based authentication opens up interesting possibilities of serving
data for different platforms with [HTTP access control (CORS)][3].

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: Authorization
```

First, setting Allow Origin header for the web application to
makes data and resources available to requests coming from any domain.
Additionally if data or resources are restricted setting  `Authorization` header
should be enabled to permit header authorizing user to the resource.

Here is an interesting and really good answer from security discussion from
[stackexchange][4] about using `Access-control-allow-origin: *`
with a bearer token.

>Authorization header is not a simple header, so would require a preflight that
results in an Access-Control-Allow-Headers response returning that header.
The server not returning this would also prevent any CSRF attack,
because the pre-flight will block it.



[1]: https://en.wikipedia.org/wiki/Cross-site_request_forgery
[2]: https://en.wikipedia.org/wiki/Man-in-the-middle_attack
[3]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS
[4]: http://security.stackexchange.com/a/128882/121159
