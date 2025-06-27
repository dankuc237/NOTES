Wireless traffic analysis tool. 
It supports multiple wireless technologies (Wi-Fi, Bluetooth, nRF, SDR), geolocation tracking, forensic log analysis, and detection of rogue APs or unauthorized devices.

**Configuration Files**
Located in `/etc/kismet/`. Key files:
- `kismet.conf`: master config
- `kismet_80211.conf`: Wi-Fi config
- `kismet_alerts.conf`: alert/WIDS
- `kismet_filter.conf`: filtering rules
- `kismet_httpd.conf`: web server config
- `kismet_logging.conf`: log settings
- `kismet_memory.conf`: memory settings
- `kismet_uav.conf`: drone detection

# kismet_site.conf
User-created override file (e.g. logging settings)

Any options we specify in this file override the settings in the default configuration files.
We can use this feature to maintain our own custom settings even if an update to Kismet includes modifications to the default configurations files.
```bash
sudo mkdir /var/log/kismet # Create logging directory
```
```bash
cat /etc/kismet/kismet_site.conf # override file

log_prefix=/var/log/kismet/
log_types=kismet,pcapng
```

# Commands
```bash
sudo kismet -c wlan0 --no-ncurses # Starting Kismet
# Automatically enters monitor mode.

sudo kismet -c wlan0:channels="4,5,6" # Channel-specific capture
sudo kismet --daemonize # Run as daemon
```
```bash
ps aux | grep kismet # stop Kismet
sudo kill -9 <PID>
```


# Web Interface

Accessible at `http://localhost:2501` after launching `Kismet`

**First login**
- Create user
- Enable `wlan0` via UI under "Data Sources"

**Features**
- Real-time device list
- Channel visualization
- Device/SSID details
- Filters

## Secure the UI:

In `kismet_site.conf`:

```conf
httpd_bind_address=127.0.0.1
```

# Remote Capture
1. Start Kismet server with no sources:
```bash
sudo kismet
```
2. On remote device:
```bash
ssh -L 8000:localhost:3501 kali@server_ip
```
3. On remote device:
```bash
sudo kismet_cap_linux_wifi --connect 127.0.0.1:8000 \
  --user offsec --password lab \
  --source=wlan0:name=remote-wlan0
```

# Log Files
- Default logs in `.kismet` (SQLite DB) and `.pcapng`
Open `.kismet` with:
```bash
$ sqlite3 /var/log/kismet/FILE.kismet
> .tables
> .schema devices
> .headers on
> SELECT type, devmac FROM devices;
> .quit

# One-liner
$ sqlite3 FILE.kismet "select type, devmac from devices;"
```
## Logging options:
`--log-prefix <dir>`
`--log-types <types>`
`--no-logging`

## Replaying Log Files
```bash
sudo kismet -c path/file.pcap:realtime=true
```
```bash
:realtime=true # plays back at real-time speed
pps=N # to control packets-per-second
```
    

## Exporting Data

```bash
kismetdb_to_pcap --in FILE.kismet --out FILE.pcapng # To PCAP

kismetdb_dump_devices --in FILE.kismet --out devices.json --skip-clean --verbose # To JSON
```
