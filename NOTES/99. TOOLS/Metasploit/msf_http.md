```bash
auxiliary/scanner/http/apache_userdir_enum
auxiliary/scanner/http/brute_dirs
auxiliary/scanner/http/dir_scanner
auxiliary/scanner/http/dir_listing
auxiliary/scanner/http/http_put
auxiliary/scanner/http/files_dir
auxiliary/scanner/http/http_login
auxiliary/scanner/http/http_header
auxiliary/scanner/http/http_version
auxiliary/scanner/http/robots_txt
```

```bash
# web application vulnerability scanner
load wmap

# trzeba korzystac w odpowiedniej kolejności
wmap_sites -a <IP>
wmap_targets -t http://IP
wmap_targets -l
wmap_run -t
wmap_run -e
wmap_vulns
```

```bash
msf5 > wmap_sites [options]
        -h        Display this help text
        -a [url]  Add site (vhost,url)
        -d [ids]  Delete sites (separate ids with space)
        -l        List all available sites
        -s [id]   Display site structure (vhost,url|ids) (level) (unicode output true/false)

msf5 > wmap_targets -h
[*] Usage: wmap_targets [options]
        -h              Display this help text
        -t [urls]       Define target sites (vhost1,url[space]vhost2,url)
        -d [ids]        Define target sites (id1, id2, id3 ...)
        -c              Clean target sites list
        -l              List all target sites

msf5 > wmap_run [options]
        -h                        Display this help text
        -t                        Show all enabled modules
        -m [regex]                Launch only modules that name match provided regex.
        -p [regex]                Only test path defined by regex.
        -e [/path/to/profile]     Launch profile modules against all matched targets.
                                  (No profile file runs all enabled modules.)
```