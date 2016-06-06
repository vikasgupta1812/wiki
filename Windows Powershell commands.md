Windows - PowerShell Commands
===

`pwd` print working directory
`hostname`my computer's network name
`mkdir`make directory
`cd`change directory
`ls`list directory
`rmdir`remove directory
`pushd`push directory
`popd`pop directory

The `pushd` command takes your current directory and **pushes** it into a list for later, then it changes to another directory. It's like saying, "Save where I am, then go here."

The popd command takes the last directory you pushed and "pops" it off, taking you back there.

```
> cd temp
> mkdir -p i/like/icecream
```

```
    Directory: C:\Users\zed\temp\i\like


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        12/20/2011  11:05 AM            icecream

```
```
> pushd i/like/icecream
> popd
> pwd
```

```
Path
----
C:\Users\zed\temp
```

```
> pushd i/like
> pwd
```

```
Path
----
C:\Users\zed\temp\i\like
```

```
> pushd icecream
> pwd
```
```
Path
----
C:\Users\zed\temp\i\like\icecream
```

```
> popd
> pwd
```
```
Path
----
C:\Users\zed\temp\i\like
```

```
> popd
>
```


`cp` copy a file or directory

```
> cd temp
> cp iamcool.txt neat.txt
> ls


    Directory: C:\Users\zed\temp


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        12/22/2011   4:49 PM          0 iamcool.txt
-a---        12/22/2011   4:49 PM          0 neat.txt


```

```
> cp neat.txt awesome.txt
> ls


    Directory: C:\Users\zed\temp


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        12/22/2011   4:49 PM          0 awesome.txt
-a---        12/22/2011   4:49 PM          0 iamcool.txt
-a---        12/22/2011   4:49 PM          0 neat.txt


```

```
> cp awesome.txt thefourthfile.txt
> ls


    Directory: C:\Users\zed\temp


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        12/22/2011   4:49 PM          0 awesome.txt
-a---        12/22/2011   4:49 PM          0 iamcool.txt
-a---        12/22/2011   4:49 PM          0 neat.txt
-a---        12/22/2011   4:49 PM          0 thefourthfile.txt


```

```
> mkdir something


    Directory: C:\Users\zed\temp


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        12/22/2011   4:52 PM            something


```

```
> cp awesome.txt something/
> ls
    Directory: C:\Users\zed\temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----        12/22/2011   4:52 PM            something
-a---        12/22/2011   4:49 PM          0 awesome.txt
-a---        12/22/2011   4:49 PM          0 iamcool.txt
-a---        12/22/2011   4:49 PM          0 neat.txt
-a---        12/22/2011   4:49 PM          0 thefourthfile.txt
```

```
> ls something
    Directory: C:\Users\zed\temp\something

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        12/22/2011   4:49 PM          0 awesome.txt

```

```
> cp -recurse something newplace
> ls newplace
    Directory: C:\Users\zed\temp\newplace
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---        12/22/2011   4:49 PM          0 awesome.txt
```


`robocopy`robust copy
`mv`move a file or directory
`more`page through a file
`type`print the whole file
`forfiles`run a command on lots of files
`dir -r`find files
`select-string`find things inside files
`help`read a manual page
`helpctr`find what man page is appropriate
`echo`print some arguments
`set`export/set a new environment variable
`exit` exit the shell
`runas` DANGER! become super user root DANGER!
`attrib` change permission modifiers
`iCACLS` change ownership