# Error: `sudo: unable to resolve host ip-xxx-xx-xx-xxx`


Issue might occur when you try to use `sudo`

Simple solution is to (1) verify host name by with:
```
cat /etc/hostname
```

Then you can edit `/etc/hosts` file and append your hostname to the top 127.0.0.1 line.

Done!
