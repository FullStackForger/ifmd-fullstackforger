# Mackports - short guide

This is super short macports guide. This article is an ongoing work in progress. I am adding more and
more stuff here every time I encounter a problem or something interesting.

If you looking for full [official guide](https://guide.macports.org), just follow the link.

## Activating, deactivating packages

<!-- http://stackoverflow.com/questions/4231228/macports-setting-an-install-as-active -->
```
sudo port deactivate portname@x.y.z-0_0+q1
sudo port activate  portname@x.y.z-0_0+q2
```

## Uninstalling packages

<!-- https://guide.macports.org/#using.port.uninstall -->
<!-- http://apple.stackexchange.com/a/10190 -->

Uninstall port with its dependencies:

```
sudo port uninstall --follow-dependencies portname
```

Uninstall old versions of ports:
```
sudo port uninstall inactive
```
It may reveal a few more leaves (i.e. ports that are dependencies of ports that are installed, but inactive).

List all unrequested ports:
```
sudo port echo leaves
```

> One of features of the 2.x version is tracking “requested” versus “unrequested” port installations. Unrequested ports are installed as a result of being a dependency for another port installation.

> `leaves` is a pseudo-portname expanding to all the unrequested ports. It can be used to “clean up” unneeded ports skipped during uninstallation (if you forgot `--follow-dependencies` flag)

> source: [http://apple.stackexchange.com/a/10190](http://apple.stackexchange.com/a/10190)

Uninstall any remaining leaves:
```
sudo port uninstall leaves
```
