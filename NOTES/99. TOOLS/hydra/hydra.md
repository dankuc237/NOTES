# hydra
```bash
hydra  
[[[-l LOGIN|-L FILE] [-p PASS|-P FILE|-x OPT -y]] | [-C FILE]]  
[-e nsr] [-u] [-f|-F] [-M FILE] [-o FILE] [-b FORMAT]  
[-t TASKS] [-T TASKS] [-w TIME] [-W TIME] [-m OPTIONS] [-s PORT]  
[-c TIME] [-S] [-O] [-4|6] [-I] [-vV] [-d]  
target service [OPTIONS]
```
**target**- a target to attack, can be an IPv4 address, IPv6 address or DNS name.  
**service**- a service to attack, see the list of protocols available


https://infinitelogins.com/2020/02/22/how-to-brute-force-websites-using-hydra/

|                          |                                                                                                                                                                                                  |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `-l LOGIN` <br>`-L FILE` | login with LOGIN name <br> load several logins from FILE                                                                                                                                          |
| `-p PASS` <br>`-P FILE`  | try password PASS, <br>load several passwords from FILE                                                                                                                                           |
| `-C FILE`                | colon separated "login:pass" format, instead of -L/-P options                                                                                                                                    |
| `-v / -V`                | verbose mode / show login+pass combination for each attempt                                                                                                                                      |
| `-I`                     | ignore an existing restore file (don't wait 10 seconds)                                                                                                                                          |
| `-R`                     | restore a previously aborted session. Requires a hydra.restore file was written. Options are restored, but can be changed by setting them after -R on the command line                           |
|`-s`|| 
| `-u`                     | by default Hydra checks all passwords for one login and then tries the next login. This option loops around the passwords, so the first password is tried on all logins, then the next password. |
| `-f`                     | exit after the first found login/password pair (per host if -M)                                                                                                                                  |
| `-F`                     | exit after the first found login/password pair for any host (for usage with -M)                                                                                                                  |
| `-M FILE`                | list of servers to attack, one entry per line, ':' to specify port                                                                                                                               |
| `-t TASKS`               | run TASKS number of connects in parallel per target (default: 16)                                                                                                                                |
| `-U`                     | service module usage details                                                                                                                                                                     |
| `-m OPT`                 | options specific for a module, see -U output for information                                                                                                                                     |
| `-h`                     | more command line options (COMPLETE HELP)                                                                                                                                                        |
| `server`                 | the target: DNS, IP or 192.168.0.0/24 (this OR the -M option)                                                                                                                                    |
| `service`                | the service to crack (see below for supported protocols)                                                                                                                                         |
| `OPT`                    | some service modules support additional input (-U for module help)                                                                                                                               |
# Basic Usage
## NTLM auth
```bash
$ hydra -L <username_list> -p <pwd_to_spray> <host> http-get '/:A=NTLM:F=401'
```
## SSH
```bash
hydra -f -l <user> -P /usr/share/wordlists/rockyou.txt <IP> -t 4 ssh
```
## HTTP Post Form 
```bash
hydra -l <user> -P /usr/share/wordlists/rockyou.txt $target http-post-form "<Login Page>:<Request Body>:[F=<ErrorMessage> lub S=<SuccesMessage>]"
```
## Http-get (WebDAV)
```bash
$ hydra -L ... -P ... <IP> http-get /web-dav/
**[**```
## Wordpress 
```bash
hydra -f -l <user> -P /usr/share/wordlists/rockyou.txt <IP> -V http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location'
```
## MySQL 
```bash
hydra -f -l <user> -P /usr/share/wordlists/rockyou.txt mysql:<IP> 
```
## FTP 
```bash
hydra -f -l <user> -P /usr/share/wordlists/rockyou.txt ftp:<IP> 
```
## SMB 
```bash
$ hydra -f -l <user> -P /usr/share/wordlists/rockyou.txt smb://<IP> # smb bruteforce user >> psexec
```
## Windows RDP 
```bash
hydra -f -l <user> -P /usr/share/wordlists/rockyou.txt rdp://<IP>
```
