
# VHOST mode (subdomains)

In vhosts mode the tool is checking if the subdomain exists by visiting the formed url and verifying the IP address.

```bash
$ gobuster vhost -w <wordlist.txt> -u <url> --append-domain -o <output_file.txt>
```
```bash
gobuster vhost --append-domain -w /usr/share/wordlists/seclists/Discovery/DNS/ -u $TARGET
```
wirtualne hosty na serwerze http (na jednym hostingu, serwerze można wyświetlać kilka stron)

# DNS mode (subdomains in dns)
enumerate subdomains on domain
tool attempts to DNS resolve the subdomains and based on that we are given the result.

```bash
$ gobuster dns -r <DNS> -d <domain> -w <wordlist_subdomains> # DNS mode
```

# BURP redirection
https://infinitelogins.com/2020/03/20/how-to-route-tools-gobuster-through-a-burpsuite-proxy/
1. Go to proxy settings
2. Add a new proxy listeners
3. Request handling tab > add Redirect to host and port
4. Make `gobuster` request to new proxy point `$ gobuster dir -u http://url.com `

# DIR mode
**better use [[feroxbuster (content discovery)]]**
```bash
$ gobuster dir -u <url> -w <wordlist_file.txt> -x <file_extensions> -e # DIR mode, expand found URLS

$ gobuster dir -u mytarget.com -w path/to/my/awesome/wordlist.txt -e -t 100 -x jpg,jpeg,png,gif,ico # find images
$ gobuster dir -u mytarget.com -w path/to/my/awesome/wordlist.txt -e -t 100 -x php,txt,html,htm # web interesting files
```

