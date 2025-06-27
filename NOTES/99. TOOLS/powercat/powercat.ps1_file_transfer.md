```PowerShell
PS> powercat -c 10.1.1.1 -p 443 -i C:\inputfile # Send File
PS> powercat -l -p 8000 -of C:\inputfile # Recieve File
PS> powercat -l -p 443 -i C:\inputfile -rep # Start A Persistent Server That Serves a File
```
![[powercat.ps1_installation]]