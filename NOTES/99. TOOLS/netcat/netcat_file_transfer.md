# $ netcat
unencrypted traffic, easily detectable by IDS
```bash
# receive and save file
$ nc -l -p 1234 > <file>

# send file 
$ nc -w 3 <IP> 1234 < <file>
```