# $ chisel
https://github.com/jpillora/chisel/releases/latest
https://exploit-notes.hdks.org/exploit/network/port-forwarding/port-forwarding-with-chisel/

# Port forwarding
```bash
# kali
$ chisel client <server_IP:port> [<local_host>]<local_port>:<serverIP:port>

# target box
$ chisel server [-p <listen_port>]
```
# Reverse Port Forwarding
```bash
# Kali
$ chisel server [-p <port>] --reverse [--keepalive 5s]# attack machine

# target box
$ chisel client <serverIP:port> <remote> [<remote>] [<remote>] ...

<remote> = R:<local-interface>:<local-port>:<remote-host>:<remote-port>/<protocol>
# which does reverse port forwarding, 
# sharing <remote-host>:<remote-port> from the client 
# to the server's <local-interface>:<local-port>

$ chisel client <serverIP:port> R:80:172.16.22.2:80 R:3000:localhost:3000
```
# reverse SOCKS proxy
```bash
$ cat /etc/proxychains.conf:
...
socks5 127.0.0.1 1080
```
```bash
# kali
$ chisel server -p 8080 --reverse

# victim
$ chisel client 10.10.14.3:8080 R:socks
```