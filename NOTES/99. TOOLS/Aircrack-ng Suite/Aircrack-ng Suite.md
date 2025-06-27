# Aircrack-ng Suite

This module introduces the **Aircrack-ng suite**, used for Wi-Fi security auditing and penetration testing. Tools include network detection, packet capture, injection, and cracking.
## Airmon-ng (enable monitor mode on wireless interfaces)
https://www.aircrack-ng.org/doku.php?id=airmon-ng
Manage monitor mode for wireless interfaces.
```bash
sudo airmon-ng # List wireless interfaces with PHY, interface name, driver, and chipset.
sudo airmon-ng --verbose / --debug # Get detailed driver, firmware, and system info.
```
```bash
sudo airmon-ng check # Detect processes that interfere with monitor mode (e.g., NetworkManager, wpa_supplicant).
sudo airmon-ng check kill # Kill interfering processes.
```
```bash
sudo airmon-ng start wlan0 # Enable monitor mode on wlan0, creating wlan0mon.
sudo airmon-ng start wlan0 3 # Same as above, but on channel 3.

sudo iw dev wlan0mon info # Confirm monitor mode and channel.

sudo airmon-ng stop wlan0mon # Disable monitor mode and return to managed mode.
```

> [!warning]+
> We only need to set the channel when the tool we use after Airmon-ng doesn't have the ability to set the channel.
> One example would be `Aireplay-ng`. In most cases, we'll run `Airodump-ng` before `Aireplay-ng`. In that case, the channel will be set by `Airodump-ng`. We will explore both tools later in this module.


## Airodump-ng (packet capture, capturing raw 802.11 frames)
https://www.aircrack-ng.org/doku.php?id=airodump-ng
Capture raw 802.11 frames and display Wi-Fi network data.
Particularly suitable for collecting WEP Initialization Vectors (IVs) or WPA/WPA2 handshakes, which use with Aircrack-ng or other 802.11 cracking tools.
```bash
sudo airodump-ng -c <channel> # Listen on specific channel.
sudo airodump-ng --bssid <BSSID> # Filter capture to a specific AP.
sudo airodump-ng -w <prefix> # Save captured packets with filename prefix.
```
```bash
sudo airodump-ng wlan0mon -c 6
sudo airodump-ng -c 3 --bssid 34:08:04:09:3D:38 -w <filename> wlan0mon
```
> [!info]
> By default, executing `airodump-ng` with the `-w` option, followed by a filename prefix writes the output to a number of formats:
> - PCAP, with a cap extension.
> - CSV, with a csv extension.
> - Kismet legacy CSV, with a kismet.csv extension.
> - Kismet legacy NetXML, with a kismet.netxml extension.
> - Log CSV, with a log.csv extension.
> 
> Two additional files are created with certain options:
> - GPS coordinates, with a gps extension when the `-g` option and a GPS device is configured. More details about this feature in a later section.
> - Initialization Vector (IVS), with a `.ivs` extension with the `--ivs` option. In this case, only the IVS file will be created. This option is only useful for WEP cracking.
> 
> To limit which file formats we generate, we can use the --output-format option followed by a comma separated list of file formats. An example command is sudo airodump-ng --output-format csv,pcap wlan0mon.

## Aireplay-ng (generating wireless traffic, test connecion)
Aireplay-ng is primarily useful for generating wireless traffic.

Aireplay-ng supports the following attacks. They are listed along with the corresponding number from the tool's documentation.

|Attack #|Attack Name|
|---|---|
|**0**|Deauthentication|
|**1**|Fake Authentication|
|2|Interactive Packet Replay|
|3|ARP Request Replay Attack|
|4|KoreK ChopChop Attack|
|5|Fragmentation Attack|
|6|Café-Latte Attack|
|7|Client-Oriented Fragmentation Attack|
|8|WPA Migration Mode Attack|
|**9**|Injection Test|
### Injection test
To **verify** that:
- Your **wireless card is in monitor mode**.
- The card **supports frame injection** (card can send 802.11 frames and receive acknowledgments from access points (APs)).
- The card can **communicate effectively** with the target AP.

In the context of aireplay-ng, the injection test checks whether your wireless 
This is crucial before performing any packet injection attacks like deauthentication, fake authentication, or ARP replay.
1. Set desired channel
	```bash
	kali@kali:~$ sudo airmon-ng start wlan0 3

	PHY	Interface	Driver		Chipset

	phy0	wlan0		ath9k_htc	Atheros Communications, Inc. AR9271 802.11n

		(mac80211 monitor mode vif enabled for [phy0]wlan0 on [phy0]wlan0mon)
		(mac80211 station mode vif disabled for [phy0]wlan0)
	```
2. Basic Injection Test (broadcast scan)
	```bash
	kali@kali:~$ sudo aireplay-ng -9 wlan0mon
	12:02:10  Trying broadcast probe requests...
	12:02:10  Injection is working!
	12:02:11  Found 2 APs
	
	12:02:12  34:08:04:09:3D:38 - channel: 3 - 'wifu'
	12:02:13  Ping (min/avg/max): 1.455ms/4.163ms/12.006ms Power: -37.63
	12:02:13  30/30: 100%
	
	12:02:13  C8:BC:C8:FE:D9:65 - channel: 2 - 'secnet'
	12:02:13  Ping (min/avg/max): 1.637ms/4.516ms/18.474ms Power: -28.90
	12:02:13  30/30: 100%
	```
	- Sends broadcast probe requests.
	- Lists nearby APs that reply.
	- Measures **response rate** and **latency**.
3. Specific Target Test
	```bash
	sudo aireplay-ng -9 -e <ESSID> -a <BSSID> wlan0mon
	```
	```bash
	kali@kali:~$ sudo aireplay-ng -9 -e wifu -a 34:08:04:09:3D:38 wlan0mon
	12:26:14  Waiting for beacon frame (BSSID: 34:08:04:09:3D:38) on channel 3
	12:26:14  Trying broadcast probe requests...
	12:26:14  Injection is working!
	12:26:16  Found 1 AP 
	
	12:26:16  Trying directed probe requests...
	12:26:16  34:08:04:09:3D:38 - channel: 3 - 'wifu'
	12:26:16  Ping (min/avg/max): 1.968ms/3.916ms/11.581ms Power: -35.73
	12:26:16  30/30: 100%
	```
	- Targets a **specific AP** by name and MAC address.
	- Ensures you can **reliably inject packets** to it.


**Card-to-Card Test**
```bash
kali@kali:~$ sudo aireplay-ng -9 -i wlan1mon wlan0mon

12:50:57  Trying broadcast probe requests...
12:50:57  Injection is working!
12:50:59  Found 2 APs

12:50:59  Trying directed probe requests...
12:50:59  34:08:04:09:3D:38 - channel: 3 - 'wifu'
12:51:00  Ping (min/avg/max): 1.735ms/4.619ms/12.689ms Power: -47.33
12:51:00  27/30:  90%

12:51:01  C8:BC:C8:FE:D9:65 - channel: 2 - 'secnet'
12:51:01  Ping (min/avg/max): 2.943ms/17.900ms/49.663ms Power: -117.10
12:51:01  29/30:  96%

12:51:01  Trying card-to-card injection...
12:51:01  Attack -0:           OK
12:51:02  Attack -1 (open):    OK
12:51:02  Attack -1 (psk):     OK
12:51:02  Attack -2/-3/-4/-6:  OK
12:51:02  Attack -5/-7:        OK
```

- Uses a **second card** to **confirm that injected packets are really transmitted** and seen on the air.
- Validates support for multiple Aireplay-ng **attack modes (0–9)**.


### Aireplay-ng Troubleshooting

Let's review a few troubleshooting tips. The following tips apply to all modes of Aireplay-ng.

**Aireplay-ng does not Inject Frames**

We'll want to ensure that we are using the correct monitor mode interface. Running iw dev `<interface>` info or iwconfig `<interface>` will show the wireless interface and its status. For devices using mac80211 drivers, the monitor mode interface is typically named wlan0mon.

**Aireplay-ng Hangs with No Output**

If we enter the command and it appears to hang with no output, this is usually because our wireless card is on a different channel number than the AP.

This could also be caused by another instance of Aireplay-ng running in background mode. If the options conflict, the second command might hang.

**interfaceXmon is on channel Y, but the AP uses channel Z**

Let's take a look at one example of this message. We might see something like "wlan0mon is on channel 1, but the AP uses channel 6". This tells us something is causing the wireless card to channel hop. The most likely cause is that we started monitor mode on the wrong channel. It could also be because we did not terminate interfering processes with airmon-ng check kill.

**Aireplay-ng General Troubleshooting Tips**

There are a few things we might try when troubleshooting in general.

To begin, we can look for deauthentication or disassociation messages during injection. These might indicate that we are not associated with the AP. Aireplay-ng will typically indicate this, but we can also observe it by using the tcpdump command `tcpdump -n -e -s0 -vvv -i <interface name>`

We can also ensure that the wireless card driver is properly patched and installed.

It's important to remember that physical location can make a difference. We'll want to make sure that we are physically close enough to the AP (but not too close). We can confirm that we're able to communicate with the specific AP by running the injection test.

We may also want to verify that our card is in monitor mode. In addition, the card needs to be configured on the same channel as the AP. We can use iw dev wlan0mon info to confirm this.

## Aircrack-ng
primary cracking tool in the suite, used for **offline attacks** on:
- WEP
- WPA/WPA2-PSK
- PMKID-based handshakes

It **does not interact** with live Wi-Fi networks during cracking — it uses `.cap` files with captured handshakes or PMKID. (see [[#Airodump-ng (packet capture, capturing raw 802.11 frames)]])

```bash
aircrack-ng -w wordlist.txt -b <BSSID> capture.cap
```

## Airdecap-ng
- Decrypt encrypted packets in `.cap` files after obtaining a WEP/WPA PSK/WPA2 PSK key to a wireless network.
- Strip 802.11 wireless headers from **unencrypted** (open) network captures

```bash
sudo airdecap-ng -b 34:08:04:09:3D:38 opennet-01.cap # removing wireless headers
```
- `-b <BSSID>`: Filters packets by AP MAC
- Produces a new file with `-dec` suffix: `opennet-01-dec.cap`

`Airdecap-ng` saved the data packets linked to `34:08:04:09:3D:38` into a new capture file, with `-dec`, for decrypted, appended to the original filename.

## Airgraph-ng
Python-based graphing tool that visualizes relationships from **Airodump-ng CSV files**.

Graph Types:
- Clients to AP Relationship (CAPR)
	- Shows which **clients** are associated with which **APs**
	- Color-coded by encryption:
	    - 🟢 WPA
	    - 🟡 WEP
	    - 🔴 Open
	    - ⚫ Unknown
	    ```bash
	    sudo airgraph-ng -o picture1.png -i dump-01.csv -g CAPR
		```
- Clients Probe Graph
	- Displays relationships between **clients** and **probed ESSIDs**
	```bash
	sudo airgraph-ng -o Picture2.png -i dump-01.csv -g CPG
	```




