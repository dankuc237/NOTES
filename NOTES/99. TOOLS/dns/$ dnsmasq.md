https://lo1.sandomierz.pl/ecg/dnsmasq-serwer-dhcp-i-dns?lang=pl

```bash
cat dnsmasq.conf
# Do not read /etc/resolv.conf or /etc/hosts
no-resolv
no-hosts

# Define the zone
auth-zone=feline.corp
auth-server=feline.corp
```
This configuration ignores the **/etc/resolv.conf** and **/etc/hosts** files and only defines the _auth-zone_ and _auth-server_ variables. These tell Dnsmasq to act as the authoritative name server for the **feline.corp** zone.

```bash
$ sudo dnsmasq -C dnsmasq.conf -d

# start the dnsmasq process with the dnsmasq.conf configuration file (-C),
# making sure it runs in "no daemon" (-d) mode so it runs in the foreground.
# We can kill it easily again later.
```
```bash
$ nslookup -all # # check DNS resolve settings
$ resolvectl # check DNS resolve settings
```