# Web Enumeration (DRAFT)

dnsenum

```bahs
dnsenum --enum example-domain.com -f  /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt 
```

DNS Zone Transfer

```bash
dig axfr @nsztm1.digi.ninja zonetransfer.me
```

brute-forcing vhost and subdomains

```bash
gobuster vhost -u http://domain.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```

```bash
gobuster dns -d domain.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt 
```

detect the presence of a WAF

```bash
wafw00f domain.com
```

Nikto

```bash
nikto -h domain.com
```

Crawling - ReconSpider

```bahs
 python3 ReconSpider.py http://domain.com
```

### Fuzzing

``bash
feroxbuster --url http://IP:PORT/godeep/ -x html,php,js,zip,bak,config,zip -w /usr/share/seclists/Discovery/Web-Content/common.txt
```

```bash
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ -e .php,.html,.txt,.bak,.js -v 
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://IP:PORT/FUZZ -recursion -ic -recursion-depth 2 -rate 500
```
> -ic stands for ignore wordlist comments

fuzzing POST request

```bash
ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v
```

Look for public exploits (exploitdb, github, google, metasploit)

