```bash
$ hydra -V -l <user> -p <passwd> rdp://<IP>[/<port>]
$ hydra -V -L <user_list> -p <passwd> rdp://<IP>[/<port>]
$ hydra -V -l <user> -P <passwd_list> rdp://<IP>[/<port>]
$ hydra -V -L <userslist> -P <passwd_list> rdp://<IP>[/<port>]
$ hydra -V -C <user:passwd_file> -M <targets_file> rdp
$ hydra -V -L <userslist> -P <passwd_list> rdp://<IP>[/<port>]
$ hydra -V -L <userslist> -P <passwd_list> -o <outfile> rdp://<IP>[/<port>]
```