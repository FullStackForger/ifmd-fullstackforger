# NPM from behind the proxy

If you connecting from behind the proxy and run 
```
npm info react --verbose
```

to get: 

```
Error message: npm info retry will retry, error on last attempt: 
Error: getaddrinfo ENOTFOUND registry.npmjs.org registry.npmjs.org:443
```

Here is what helped me. Let's start with:

```
nslookup registry.npmjs.org
```

It should produce something like this
```
Server:		172.20.10.1
Address:	172.20.10.1#53

Non-authoritative answer:
registry.npmjs.org	canonical name = a.sni.fastly.net.
a.sni.fastly.net	canonical name = prod.a.sni.global.fastlylb.net.
Name:	prod.a.sni.global.fastlylb.net
Address: 151.101.60.162
```

Grab the address (151.101.60.162) and add it to `/etc/hosts`

```
151.101.32.162 registry.npmjs.org
```

I also added registry to npm config.
However, this shoud be optional if you haven't modified config yourself.

Done. You should be able to use your npm again.