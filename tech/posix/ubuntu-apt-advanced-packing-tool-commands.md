# Ubuntu APT (Advanced Packing Tool)

## `apt-get`

It is the command-line tool for handling packages. It works as back-end which API is used by interfaces like _dselect_, _aptitude_, _synaptic_ or _wajig_.

**Installing package**

```
apt-get install <package>
```

**Installing package simulation** to preview system changes that occur if package is installed

```
apt-get -s install <package>
```

**Updating package index**, which is database of all packages from the repositories stored in the `/etc/apt/sources.list` file.

```
sudo apt-get update
```

**Packages upgrading** should be done after updating (command above)

```
sudo apt-get upgrade
```

**Package upgrade simulation** can be run by executing above with `-s` option

```
apt-get -V -s upgrade
```

**Removing package** from the system

```
apt-get remove <package>
```


**Removing package with its configuration file**

```
apt-get purge <package>
```

## `apt-show-versions`

_Lists available package versions with distribution_

**Previewing versions** of one specific or all packages

```
apt-show-versions <package>
```

**Listing only upgradable packages**

```
apt-show-versions -u
```

## `add-apt-repository`

Adds a repository into the `/etc/apt/sources.list` or `/etc/apt/sources.list.d` or removes an existing one

**Adding repository**

```
add-apt-repository ppa:<repository>
```

## `apt-cache`

_Provides operations to search and generate custom output from the package metadata_


Listing names of all packages
```
apt-cache pkgnames
```

Listing names of packages matching package partial
```
apt-cache pkgnames |grep <package_partial>
```

Show general information about package

```
apt-cache showpkg <package>
```

Shows package policy settings showing installed version and latest available version package can be upgrade to
```
apt-cache policy <package>
```

## sources

https://en.wikipedia.org/wiki/Advanced_Packaging_Tool
