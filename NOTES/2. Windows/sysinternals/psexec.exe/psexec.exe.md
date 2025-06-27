**Ports:** 445/TCP (SMB)
**Required Group Memberships:** Administrators
t’s intended to replace telnet-like applications and provide remote executionof processes on other systems through an interactive console.
https://learn.microsoft.com/en-us/sysinternals/downloads/psexec
It allows an administrator user to run commands remotely on any PC where he has access.

```mermaid
sequenceDiagram
    participant attacker
    participant target

    attacker -) target: Connect to `Admin$` share and upload a service binary.<br>Psexec uses `psexesvc.exe` as the name.
    target -) attacker: Connect to the service control manager<br>to create and run a service named `PSEXESVC`<br>and associate the service binary<br>with `C:\Windows\psexesvc.exe`.
    attacker -) target: Create some named pipes<br>to handle `stdin/stdout/stderr`.
```
```powershell
cmd> ./PsExec64.exe -i  \\FILES04 -u <domain>\<username> -p <passwd> cmd
```