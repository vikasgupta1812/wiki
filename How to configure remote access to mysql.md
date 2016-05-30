How to configure remote access to mysql
====

http://stackoverflow.com/questions/14779104/how-to-allow-remote-connection-to-mysql

```sql
GRANT ALL PRIVILEGES ON *.* 
TO 'root'@'%' 
IDENTIFIED BY 'password' 
WITH GRANT OPTION;
```


And then find the following line and comment it out in your my.cnf file, which usually lives on `/etc/mysql/my.cnf` on `Unix/OSX` systems. If it's a Windows system, you can find it in the MySQL installation directory, usually something like `"C:\Program Files\MySQL\MySQL Server 5.5\"` and the filename will be `my.ini`

```sql
 bind-address = 127.0.0.1
```


	./configure
	make