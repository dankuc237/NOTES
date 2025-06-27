forward proxy - single point of exit from network
reverse proxy - single point of enterance to network

# Spawning TTY Shells
https://book.hacktricks.xyz/generic-methodologies-and-resources/shells/full-ttys

[[(reverse) shell#Upgrade dummy shell]]
# Command Shell to Meterpreter
# SSH
SSH Tunnel JumpProxy (multiple jumps): https://unix.stackexchange.com/questions/744495/ssh-tunnel-through-proxychains

połączenie ssh
https://unix.stackexchange.com/questions/23291/how-to-ssh-to-remote-server-using-a-private-key
```bash
ssh -p 1234 user@IP/Hostaname
```

| |                                                                                                                                                                         |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-i a.key` | klucz prywatny użytkownika utworzony na hoście zdalnym (jeśli istnieje to można go skopiować do siebie, oddzielny plik, zmienić chmod 600, dodać jako argument do ssh ) |
| `-p`       | port na którym się połączyć                                                                                                                                             |

można się zalogować nie wykonując .bashrc (plik uruchamiany przy starcie nowego terminala)
```bash
# wygenerować u siebie klucz RSA
(kck@kck)$ ssh-keygen -f <id_rsa>   
# nadać odpowiednie uprawnienia plikom
(kck@kck)$ chmod 600 ~/.ssh/nazwa (id_rsa domyślnie)
(kck@kck)$ chmod 600 ~/.ssh/nazwa.pub (id_rsa.pub domyślnie)
(kck@kck)$ chmod 700 ~/.ssh
# umieścić swój klucz publiczny w  ofiary pliku
(nazwa_ofiary@ofiara)$ nano ~/.ssh/authorized_keys 
# wykorzystanie klucza prywatnego do logowania bez hasła
(kck@kck)$ ssh -i <id_rsa> nazwa_ofiary@ofiara 
```

# Reverse shell
https://github.com/lukechilds/reverse-shell  
reverse shell cheat sheet: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md

## Reverse Shell as a Service
https://reverse-shell.sh/yourip:port 

# Upgrade dummy shell
różne metody spawnu powłoki (nie tylko python): http://netsec.ws/?p=337  
pełen upgrade https://null-byte.wonderhowto.com/how-to/upgrade-dumb-shell-fully-interactive-shell-for-more-flexibility-0197224/

```bash
echo $TERM # own terminal info, zapisać jaki terminal
stty -a # own terminal info, zapisać ilość kolumn i wierszy
stty raw -echo && fg # allow to pass keyboard shortcuts through
```
```bash
#VICTIM
#1
python3 -c 'import pty; pty.spawn("/bin/bash")'
#2
SHELL=/bin/bash script -q /dev/null

export TERM=xterm-256color;export SHELL=bash;stty rows 40 cols 200

*CTRL-Z*

#HACKER
stty raw -echo && fg

# ENTER
```

# Escape from Restricted Shells