```PowerShell
PS> (21,22,80,443) | % {powercat -c 10.1.1.10 -p $_ -t 1 -Verbose -d} # Basic TCP Port Scanner
```
![[powercat.ps1_installation]]