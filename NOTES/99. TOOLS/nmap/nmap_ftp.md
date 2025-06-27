```bash
$ nmap <IP> -p 21 
$ nmap <IP> -p 21 --script ftp-anon # Checks if FTP server allows anonymous logins
$ nmap <IP> -p 21 --script ftp-brute --script-args userdb=/usr/share/usernames [passdb=...] # brute force password
```
