[examples explained](https://www.openwall.com/john/doc/EXAMPLES.shtml)

[cheatsheet1](https://cheatsheet.haax.fr/passcracking-hashfiles/john_cheatsheet/)
[cheatsheet2](https://countuponsecurity.files.wordpress.com/2016/09/jtr-cheat-sheet.pdf)

|            |                                      |
| ---------- | :----------------------------------- |
|`/usr/etc/john/john.conf`|`john` config file|
| `~/.john/john.pot` | cracked hashes db |
| `~/.john/john.log` | john log file|


```bash
$ hashid -j [<hash>]# Identify hashes

# crack hashes in file
$ john <hashfile> [--wordlist=wordlist] [--format=(use hashid command)] [--rules:best64]

# Retrieve the cracked passwords from db
$ john --show <hashfile> 

# mutate wordlist and output candidate passwords
# first add the rules you want in the /etc/john/john.conf file inside the rules module [List.Rules:Wordlist] to modify your wordlists
$ john --stdout --wordlist=wordlist --rules:<rule_name/hashcat/best64>

```

# add custom rule to john
```bash
# to add rule just paste content of ssh.rule file to /etc/john/john.conf'
$ cat ssh.rule
[List.Rules:<NewRuleName>]
c $1 $3 $7 $!
c $1 $3 $7 $@

[List.Rules:Append1] 
$[1
...
```