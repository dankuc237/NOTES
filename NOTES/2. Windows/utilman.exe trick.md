https://mytekrescue.com/how-to-reset-the-password-on-almost-any-windows-computer/

F12
choose device (USB with bootable iso)
"Windows Setup" Window > Next > Repair Your Computer > Troubleshoot > Command Prompt
```powershell
cd C:\Windows\System32
# change cmd to utilman
ren cmd.exe utilman2.exe
ren utilman.exe cmd.exe
ren utilman2.exe utilman.exe
```
Close cmd > Continue restarting machine
On login screen click `Ease of Access` in right-down corner
BOOM!!!


```powershell
net user username passwd /add # create user
netplwiz # add permissions
```