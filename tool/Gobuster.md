# Gobuster: Fast Web and DNS Content Discovery
Gobuster is a fast, flexible tool for brute-forcing URIs (directories and files), virtual hosts, and DNS subdomains. It's written in Go and optimized for speed and concurrency, making it a staple in CTFs and web reconnaissance during authorized penetration tests.

This post covers installation, common modes, practical examples, and tips to help you use Gobuster effectively in CTF and lab environments.

## Table of Contents
- [Installation](#installation)
- [Modes of Operation](#modes-of-operation)
- [Common Flags](#common-flags)
- [Directory and File Discovery](#directory-and-file-discovery)
- [Virtual Host Discovery (vhost)](#virtual-host-discovery-vhost)
- [DNS Subdomain Enumeration](#dns-subdomain-enumeration)
- [Extensions, Status Codes and Filtering](#extensions-status-codes-and-filtering)
- [Practical Examples](#practical-examples)
- [Tips and Best Practices](#tips-and-best-practices)

## Installation
### Linux (Debian/Ubuntu)
```
sudo apt update
sudo apt install gobuster
```
### From source (Go required)
```
go install github.com/OJ/gobuster/v3@latest
```
### MacOS (Homebrew)
```
brew install gobuster
```

Wordlists (recommended): [SecLists](https://github.com/danielmiessler/SecLists) — use `SecLists/Discovery/*` for wordlists.

## Modes of Operation

- `dir` : Directory/file brute forcing (HTTP/HTTPS)  
- `dns` : DNS subdomain enumeration (uses wordlist)  
- `vhost`: Virtual host (Host header) discovery  
- `fuzz`: Generic fuzzing with custom payloads (via stdin)

## Common Flags

```

-u, --url         Target URL (for dir/vhost/fuzz)
-w, --wordlist    Path to wordlist
-t, --threads     Number of concurrent threads (default 10)
-x, --extensions  Comma-separated extensions (e.g. php,html)
-s, --statuscodes Comma-separated expected status codes (e.g. 200,204,301,302,307,401,403)
-o, --output      Save output to file
--wildcard        Enable wildcard detection (handle catch-all responses)
--timeout         Request timeout in seconds
--proxy           Use proxy (e.g. [http://127.0.0.1:8080](http://127.0.0.1:8080))
--no-status       Do not print status codes (clean output)

```

## Directory and File Discovery

Basic directory brute-force:

```

gobuster dir -u [http://example.com](http://example.com) -w /path/to/wordlist.txt

```

Include file extensions (common in CTFs):

```

gobuster dir -u [http://example.com](http://example.com) -w /usr/share/wordlists/dirb/common.txt -x php,txt,html -t 50

```

Handle sites that return permissive status codes (wildcard):

```

gobuster dir -u [http://vulnerable.local](http://vulnerable.local) -w wordlist.txt --wildcard

```

## Virtual Host Discovery (vhost)

Discover virtual hosts via Host header:

```

gobuster vhost -u [http://example.com](http://example.com) -w /path/to/vhosts.txt

```

Useful for applications hosting multiple sites on one IP.

## DNS Subdomain Enumeration

Enumerate DNS subdomains:

```

gobuster dns -d example.com -w /path/to/subdomains.txt

```

Use a custom DNS server (e.g., internal resolver):

```

gobuster dns -d example.com -w subdomains.txt --dns-server 8.8.8.8

```

## Extensions, Status Codes and Filtering

Only show specific status codes (reduce noise):

```

gobuster dir -u [http://app.local](http://app.local) -w common.txt -s 200,301,302,403

```

Append extensions and search for files:

```

gobuster dir -u [http://app.local](http://app.local) -w wordlist.txt -x php,asp,aspx,js -t 100

```

If the server returns custom responses for missing pages, enable `--wildcard` and use a larger wordlist to confirm real hits.

## Practical Examples

1. Fast directory scan with common list and extensions:

```

gobuster dir -u [http://10.10.10.10](http://10.10.10.10) -w /usr/share/seclists/Discovery/Web-Content/common.txt -x php,html,txt -t 50 -s 200,301,302,403 -o gobuster-dir.txt

```

2. Discover admin panels and common files:

```

gobuster dir -u [https://target](https://target) -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt -x php,asp,aspx,aspx.cshtml -t 40

```

3. DNS enumeration against a domain:

```

gobuster dns -d corp.local -w subdomains.txt --dns-server 10.0.0.1 -t 50 -o gobuster-dns.txt

```

4. Virtual host discovery with a vhost wordlist:

```

gobuster vhost -u [http://10.10.10.10](http://10.10.10.10) -w vhosts.txt -t 30 -o vhosts.txt

```

5. Piping custom payloads into fuzz mode:

```

printf "admin\nlogin\napi\nwp\n" | gobuster fuzz -u [http://target/FUZZ](http://target/FUZZ) -t 40 -s 200,301,302

```

## Tips and Best Practices

- Use sensible thread counts; high concurrency is fast but can overwhelm targets or your network.  
- Start with smaller wordlists (e.g., `common.txt`) then escalate to larger lists if needed.  
- Combine Gobuster with Nmap: use Nmap to identify services/ports, then run Gobuster against discovered web services.  
- Use `--proxy` to route through Burp Suite while investigating responses.  
- Save outputs (`-o`) and filter later with `grep`/`jq`/`awk` for automation in CTF workflows.  
- Respect targets: only run Gobuster against systems you are authorized to test. Misuse may be illegal.  
- For evasive environments, randomize user-agent headers and add delays (use proxies or Burp).  
- Maintain an up-to-date wordlist collection (SecLists is highly recommended).

Gobuster is lightweight, fast, and extremely useful in reconnaissance phases — mastering its flags and integrating it into your workflow will speed up finding interesting web assets during CTFs and authorized engagements.

