# debian-package-neofetch


## To build this debian package

Install dependencies
```
apt install devscripts
```

Clone debian package sources:
```
git clone https://salsa.debian.org/debian/neofetch.git debian-package-neofetch
cd debian-package-neofetch
```


Add the official upstream
```
git remote add neofetch https://github.com/dylanaraps/neofetch
git fetch neofetch
git merge neofetch/master
```

or my patches
```
git remote add wget https://github.com/wget/neofetch
git fetch wget
git merge wget/release
```

This opens the default editor. Commit the merge.



Change the `debhelper` version in `debian/control`:

Change
```
Build-Depends: debhelper (>= 11)
```

to
```
Build-Depends: debhelper (>= 10)
```
 
Let's use `dpkg-buildpackage` instead of having to deal with `git-deborig` and the usual Debian package bureaucracy.

Edit `debian/changelog` and add your changes, keeping the previous version from upstream, but suffixing it by your own version number. In my example, `-wget-1` in order to showcase the package is the first revision from my repository.

```
neofetch (5.0.0-1-wget-1) unstable; urgency=medium

  * Add non merged patches.

 -- William Gathoye <william+debian@gathoye.be>  Wed, 18 Jul 2018 22:34:11 +0200
```

Run 

```
dpkg-buildpackage -b
```

Please note the changes we performed won't likely pass the conditions to get the package merged in the official Debian repository. This is just a rapid fix to get the package deployed on our own machines.

`scp` the package to our repo and add it using `reprepro`:

```
reprepro -VV  -b /home/wget/repo/ includedeb stretch /tmp/neofetch_5.0.0-1-wget-1_all.de
```

If `reprepro` complains for any reason, just remove the package and readd it just after:
```
reprepro -VV  -b /home/wget/repo/ remove stretch neofetch
```

Even during the removal process, you are likely to get asked your GPG passphrase.

## License

GPL v2
