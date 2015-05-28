
Create an archive with current date and time in file name using 7z command line

http://stackoverflow.com/questions/15051694/create-an-archive-with-current-date-and-time-in-file-name-using-7z-command-line


```
C:\>"C:\Program Files\7-Zip\7z" a -r "D:\FC3\%DATE:~7,2%.%DATE:~4,2%.%DATE:~-4% %TIME:~0,2%.%TIME:~3,2%.%TIME:~-5% Backup".7z  "C:\ProgramData\Orbit\46"
```

```
C:\>"C:\Program Files\7-Zip\7z" a -r "D:\Tomcat6\webapps\RelianceProd_%DATE:~7,2%.%DATE:~4,2%.%DATE:~-4% %TIME:~0,2%.%TIME:~3,2%.%TIME:~-5% Backup".7z  "D:\Tomcat6\webapps\RelianceProd"
```
