How to keep Homebrew Updated 

Source - https://www.safaribooksonline.com/blog/2014/03/18/keeping-homebrew-date/

1- Update -  newest version of Homebrew and that it has the newest list of formulae available from the main repository

```
$ brew update
Already up-to-date.
```

2- Know What You Have Installed

```
$ brew list
lynx	openssl	wget
```

3- Uninstall Unused Software
```
$ brew uninstall wget
Uninstalling /usr/local/Cellar/wget/1.15...
```

4- Check Your Used Software - See what's out of date 

```
$ brew outdated
```

5- Update Your Out-of-date Software

```
$ brew upgrade
```
You may not want to upgrade certain packages

```
$ brew pin mysql
```

upgrade specific packages 

```
$ brew upgrade wget
```
6- 