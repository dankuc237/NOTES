# Wireshark at the Command Line & Advanced Usage
## Channel Hopping
Wireshark doesn't _channel hop_. It will stay on whatever channel the wireless adapter is currently on. To quickly scan all channels on 2.4GHz, we can run the following shell script in the background in a terminal.
```
for channel in 1 6 11 2 7 10 3 8 4 9 5
do
  iw dev wlan0mon set channel ${channel}
  sleep 1
done
```

## Capture From Terminal
```bash
sudo wireshark -D # List interfaces
sudo wireshark -i wlan0mon -k # Capture on specific interface
sudo wireshark -i 4 -k # Use index instead of name
sudo wireshark -i wlan0 -I -k # Enable monitor mode
```
```bash
# Output Raw Packets to STDOUT
sudo tcpdump -i wlan0mon -w - -U
sudo dumpcap -i wlan0mon -w - -P 
sudo tshark -i wlan0mon -w -
# -w - <- write to STDOUT not file
# -U - output packet as fast as it arrives
# -P - output the data in PCAP format

# unnamed pipe them to Wireshark
sudo tcpdump -U -w - -i wlan0mon | wireshark -k -i -
# named pipe them to Wireshark
mkfifo /tmp/named_pipe
sudo tcpdump -U -w - -i wlan0mon > /tmp/named_pipe
sudo wireshark -k -i /tmp/named_pipe
```
## Remote Capture
### Terminal
```bash
# Remote SSH Capture
ssh user@host "sudo tcpdump -U -w - -i wlan0mon" | sudo wireshark -k -i -
```
### Graphical
1. ![[Pasted image 20250508090100.png]]
2. Click on the cog wheel to the left of _SSH remote capture: sshdump_ at the bottom of the _Capture_ section to open the options window.

The user on the remote system must be able to either initiate a capture or have access to sudo. If it's the latter, check the _Use sudo on the remote machine_ checkbox.

> [!info]+
>When _Save parameter(s) on capture start_ is checked, the next time SSHdump is used, it won't prompt for settings and will start automatically. If the settings are not properly set and an error results, Wireshark does not make resetting to the defaults easy. They can be reset via _Edit_ > _Preferences..._ > _Advanced_. In the resulting _Search:_ textbox, we type "sshdump". Then double click every modified parameter (anything in bold) to set SSHDump back to the default values. Click on _OK_ and SSHDump is back to its default configuration.

# WEP/WPA Decryption
**Enable in GUI:** `Preferences > Protocols > IEEE 802.11 > Enable decryption`

Add Keys: 
- WEP: `wep:<hex_key>`
- WPA passphrase: `wpa-pwd:<passphrase>:<SSID>`
- WPA PMK: `wpa-psk:<hex_pmk>`
- Generate PMK:
```bash
$ wpa_passphrase <SSID> <passphrase>
	
kali@kali:~$ wpa_passphrase test abcdefg:
network={
ssid="test"
#psk="abcdefg:"
psk=a1c425c0f4e5ff3746920c90cc55d17f4773512b6c1ed415526a3bcea3351b5b
}
```
	
![[2be89e9eb9e3b3c0843fd6ca60f4dac2-ws_adding_wpa_psk2.png]]




