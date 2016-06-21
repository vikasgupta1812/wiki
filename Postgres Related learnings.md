### How to start postgreSQL server on mac os x?

```bash
brew info postgres
```

```
brew services start postgresql
```

```
postgres -D /usr/local/var/postgres
```


How to connect through shell 


```bash
psql -d mygmail
```

```
mygmail=# 
```

## Alter table column length. 
```sql
ALTER TABLE mytable ALTER COLUMN mycolumn TYPE varchar(40);
```


- Change postgres default template0 to UTF8 encoding https://gist.github.com/ffmike/877447

- PostgreSQL 9.2 upgrade steps https://gist.github.com/joho/3735740

