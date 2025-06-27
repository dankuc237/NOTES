https://www.hackingarticles.in/a-detailed-guide-on-feroxbuster/
# Quick explained
```bash
$ feroxbuster \
	--request-file <FILE> /  -u <URL> 
	-w <FILE> # wordlist
	--insecure # no cert validation
	-o / --output results.txt
	
	-r # follow redirects
	-n # no recursion
	--resume-from <STATE_FILE>
	-H <HEADER>
	-m <HTTP_METHODS> # Which method(s) should be sent (default: GET)
	-x <FILE_EXTENSION>
```

# filters
```bash

# match code
-s, --status-codes <STATUS_CODE>

# filter out
-X, --filter-regex <REGEX>
-C, --filter-status <STATUS_CODE>
-S, --filter-size <SIZE>
-W, --filter-words <WORDS>
-N, --filter-lines <LINES>
```
|Burp cooperation||
|-|-|
|`--burp`|`--proxy http://127.0.0.1:8080 --insecure true`|
|`--burp-replay`|`--replay-proxy http://127.0.0.1:8080 --insecure true`|
