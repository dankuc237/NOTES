
````bash
portfwd list
portfwd flush
portfwd add 
        -l 3389 # local port that will be listening and forwarded to our target
        -p 3389 # destination port on our targeting host
        -r [target host] # targeted system’s IP or hostname
portfwd delete –l 3389