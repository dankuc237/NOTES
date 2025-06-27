# Crack Password Manager / KeePass
```bash
$ keepass2john CrackThis.kdb > CrackThis.txt # extract hash

$ hashcat --help | grep -i "KeePass"
>  13400 | KeePass 1 (AES/Twofish) and KeePass 2 (AES)      | Password Managers

$ hashcat -a 0 -m 13400 -o cracked_output.txt --outfile-format 2 CrackThis.hash rockyou.txt
```