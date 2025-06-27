```bash
# listen on port 2222 and forward to 10.4.198.215:22
$ socat TCP4-LISTEN:2222,fork TCP4:10.4.198.215:22 
$ socat -ddd  TCP-LISTEN:2222,fork TCP4:10.4.198.215:22
```