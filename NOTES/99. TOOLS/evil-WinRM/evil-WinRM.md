creates shell with WinRM
```bash
$ evil-winrm -i <IP> -u <user> -p <passwd> # login with WinRM
$ evil-winrm -i <IP> -u <user> -p <passwd> -P 5986 # -P: Specifify port
$ evil-winrm -i <IP> -u <user> -p <passwd> -s /opt/scripts # PowerShell Local Path (-s)
$ evil-winrm -i <IP> -u <user> -p <passwd> -S # SSL enabled (-S)
```